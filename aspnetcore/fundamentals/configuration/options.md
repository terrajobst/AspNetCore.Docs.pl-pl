---
title: Wzorzec opcji w ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać wzorca opcji do reprezentowania grup powiązanych ustawień w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
uid: fundamentals/configuration/options
ms.openlocfilehash: 1f3625380d816c7d4df5a7a24b0ac146500330de
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447208"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="b2fb0-103">Wzorzec opcji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2fb0-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="b2fb0-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b2fb0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b2fb0-105">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b2fb0-106">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b2fb0-107">[Zasady podziału interfejsu (ISP) lub &ndash; hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b2fb0-108">[Separacja problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień różnych części aplikacji nie zależy od siebie ani nie jest połączona.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b2fb0-109">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="b2fb0-110">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="b2fb0-111">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b2fb0-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="b2fb0-112">Pakiet</span><span class="sxs-lookup"><span data-stu-id="b2fb0-112">Package</span></span>

<span data-ttu-id="b2fb0-113">Pakiet [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) jest niejawnie przywoływany w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="b2fb0-114">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-114">Options interfaces</span></span>

<span data-ttu-id="b2fb0-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="b2fb0-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="b2fb0-117">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="b2fb0-117">Change notifications</span></span>
* [<span data-ttu-id="b2fb0-118">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="b2fb0-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="b2fb0-119">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="b2fb0-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="b2fb0-120">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="b2fb0-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="b2fb0-121">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="b2fb0-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="b2fb0-123">Ma ona jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="b2fb0-124">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="b2fb0-125">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="b2fb0-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="b2fb0-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="b2fb0-128">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="b2fb0-129">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="b2fb0-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="b2fb0-131">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="b2fb0-132"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="b2fb0-133">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="b2fb0-134">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="b2fb0-135">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="b2fb0-135">General options configuration</span></span>

<span data-ttu-id="b2fb0-136">Konfiguracja opcji ogólnych jest przedstawiona jako przykład 1 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-136">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="b2fb0-137">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b2fb0-138">Następująca Klasa `MyOptions`ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b2fb0-139">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b2fb0-140">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b2fb0-141">Klasa `MyOptions` zostanie dodana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b2fb0-142">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b2fb0-143">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="b2fb0-144">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="b2fb0-145">W przypadku używania <xref:System.Configuration.ConfigurationBuilder> niestandardowego do załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="b2fb0-146">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b2fb0-147">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="b2fb0-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="b2fb0-148">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład 2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-148">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="b2fb0-149">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-149">Use a delegate to set options values.</span></span> <span data-ttu-id="b2fb0-150">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b2fb0-151">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b2fb0-152">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b2fb0-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b2fb0-154">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="b2fb0-155">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="b2fb0-156">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="b2fb0-157">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="b2fb0-158">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są przesłonięte przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b2fb0-159">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b2fb0-160">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b2fb0-161">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-161">Suboptions configuration</span></span>

<span data-ttu-id="b2fb0-162">Konfiguracja podopcji jest przedstawiana jako przykład 3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-162">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="b2fb0-163">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="b2fb0-164">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b2fb0-165">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b2fb0-166">Na przykład właściwość `MyOptions.Option1` jest powiązana z `Option1`klucza, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b2fb0-167">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b2fb0-168">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b2fb0-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b2fb0-169">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="b2fb0-170">Plik *appSettings. JSON* przykładu definiuje `subsection` składową z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b2fb0-171">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`do przechowywania wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b2fb0-172">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b2fb0-173">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="b2fb0-174">Iniekcja opcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-174">Options injection</span></span>

<span data-ttu-id="b2fb0-175">Iniekcja opcji jest przedstawiana jako przykład 4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-175">Options injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="b2fb0-176">Wsuń <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="b2fb0-177">Strona Razor lub widok MVC z dyrektywą [`@inject`](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-177">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="b2fb0-178">Model strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-178">A page or view model.</span></span>

<span data-ttu-id="b2fb0-179">Poniższy przykład z przykładowej aplikacji wprowadza <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do modelu strony (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b2fb0-180">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` za pomocą dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="b2fb0-181">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-181">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b2fb0-183">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b2fb0-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b2fb0-184">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie 5 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="b2fb0-185">Przy użyciu <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>opcje są obliczane raz dla każdego żądania, gdy jest on używany i buforowany przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="b2fb0-186">Różnica między `IOptionsMonitor` i `IOptionsSnapshot` to:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="b2fb0-187">`IOptionsMonitor` to [Pojedyncza usługa](xref:fundamentals/dependency-injection#singleton) , która pobiera bieżące wartości opcji w dowolnym momencie, która jest szczególnie przydatna w pojedynczych zależnościach.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="b2fb0-188">`IOptionsSnapshot` jest [usługą objętą zakresem](xref:fundamentals/dependency-injection#scoped) i zawiera migawkę opcji w czasie konstruowania obiektu `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="b2fb0-189">Migawki opcji są przeznaczone do użycia z zależnościami przejściowymi i zakresowymi.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="b2fb0-190">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b2fb0-191">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b2fb0-192">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b2fb0-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b2fb0-193">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b2fb0-194">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b2fb0-195">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b2fb0-196">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="b2fb0-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b2fb0-197">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład 6 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="b2fb0-198">Obsługa "nazwanych opcji" pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-198">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b2fb0-199">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Skonfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="b2fb0-200">W nazwanych opcjach jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-200">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b2fb0-201">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-201">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b2fb0-202">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-202">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b2fb0-203">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-203">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b2fb0-204">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-204">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b2fb0-205">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-205">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b2fb0-206">Wartość domyślna dla `Option2` dostarczonej przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-206">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="b2fb0-207">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="b2fb0-207">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="b2fb0-208">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-208">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="b2fb0-209">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-209">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="b2fb0-210">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-210">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b2fb0-211">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-211">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b2fb0-212">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-212">All options are named instances.</span></span> <span data-ttu-id="b2fb0-213">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-213">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b2fb0-214"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-214"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="b2fb0-215">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-215">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="b2fb0-216">Opcja `null` nazwana służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-216">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="b2fb0-217">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="b2fb0-217">OptionsBuilder API</span></span>

<span data-ttu-id="b2fb0-218"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-218"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="b2fb0-219">`OptionsBuilder` usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego wywołania `AddOptions<TOptions>(string optionsName)`, a nie pojawiają się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-219">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="b2fb0-220">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-220">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="b2fb0-221">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-221">Use DI services to configure options</span></span>

<span data-ttu-id="b2fb0-222">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-222">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="b2fb0-223">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-223">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="b2fb0-224">`OptionsBuilder<TOptions>` zapewnia przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które zezwalają na użycie maksymalnie pięciu usług w celu skonfigurowania opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-224">`OptionsBuilder<TOptions>` provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="b2fb0-225">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-225">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="b2fb0-226">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-226">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="b2fb0-227">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-227">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="b2fb0-228">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-228">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="b2fb0-229">Sprawdzanie poprawności opcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-229">Options validation</span></span>

<span data-ttu-id="b2fb0-230">Sprawdzanie poprawności opcji umożliwia sprawdzenie opcji w przypadku skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-230">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="b2fb0-231">Wywołaj `Validate` z metodą walidacji, która zwraca `true`, jeśli opcje są prawidłowe i `false` jeśli są nieprawidłowe:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-231">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="b2fb0-232">Poprzedni przykład ustawia nazwane wystąpienie opcji na `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-232">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="b2fb0-233">Domyślne wystąpienie opcji jest `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-233">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="b2fb0-234">Walidacja jest uruchamiana po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-234">Validation runs when the options instance is created.</span></span> <span data-ttu-id="b2fb0-235">Wystąpienie opcji gwarantuje przekazanie walidacji przy pierwszym dostępie do niego.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-235">An options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b2fb0-236">Sprawdzanie poprawności opcji nie chroni przed modyfikacjami opcji po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-236">Options validation doesn't guard against options modifications after the options instance is created.</span></span> <span data-ttu-id="b2fb0-237">Na przykład opcje `IOptionsSnapshot` są tworzone i weryfikowane raz na żądanie po pierwszym uzyskaniu dostępu do tych opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-237">For example, `IOptionsSnapshot` options are created and validated once per request when the options are first accessed.</span></span> <span data-ttu-id="b2fb0-238">Opcje `IOptionsSnapshot` nie są weryfikowane ponownie przy kolejnych próbach dostępu *dla tego samego żądania*.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-238">The `IOptionsSnapshot` options aren't validated again on subsequent access attempts *for the same request*.</span></span>

<span data-ttu-id="b2fb0-239">Metoda `Validate` akceptuje `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-239">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="b2fb0-240">Aby w pełni dostosować walidację, zaimplementuj `IValidateOptions<TOptions>`, co umożliwia:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-240">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="b2fb0-241">Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="b2fb0-241">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="b2fb0-242">Walidacja zależna od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="b2fb0-242">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="b2fb0-243">`IValidateOptions` sprawdza poprawność:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-243">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="b2fb0-244">Konkretne nazwane wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-244">A specific named options instance.</span></span>
* <span data-ttu-id="b2fb0-245">Wszystkie opcje, gdy `name` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-245">All options when `name` is `null`.</span></span>

<span data-ttu-id="b2fb0-246">Zwróć `ValidateOptionsResult` z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-246">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="b2fb0-247">Weryfikacja oparta na adnotacji danych jest dostępna w pakiecie [Microsoft. Extensions. options. DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) , wywołując metodę <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> w `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-247">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="b2fb0-248">`Microsoft.Extensions.Options.DataAnnotations` jest niejawnie przywoływana w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-248">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

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

<span data-ttu-id="b2fb0-249">Eager sprawdzanie poprawności (niepowodzenie szybkie przy uruchamianiu) jest rozważane dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-249">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="b2fb0-250">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-250">Options post-configuration</span></span>

<span data-ttu-id="b2fb0-251">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-251">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="b2fb0-252">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-252">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b2fb0-253"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępny do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-253"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b2fb0-254">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-254">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="b2fb0-255">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b2fb0-255">Accessing options during startup</span></span>

<span data-ttu-id="b2fb0-256"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-256"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="b2fb0-257">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-257">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b2fb0-258">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-258">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="b2fb0-259">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-259">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b2fb0-260">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-260">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b2fb0-261">[Zasady podziału interfejsu (ISP) lub &ndash; hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-261">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b2fb0-262">[Separacja problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień różnych części aplikacji nie zależy od siebie ani nie jest połączona.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-262">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b2fb0-263">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-263">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="b2fb0-264">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-264">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="b2fb0-265">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b2fb0-265">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b2fb0-266">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b2fb0-266">Prerequisites</span></span>

<span data-ttu-id="b2fb0-267">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-267">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="b2fb0-268">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-268">Options interfaces</span></span>

<span data-ttu-id="b2fb0-269"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-269"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="b2fb0-270"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-270"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="b2fb0-271">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="b2fb0-271">Change notifications</span></span>
* [<span data-ttu-id="b2fb0-272">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="b2fb0-272">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="b2fb0-273">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="b2fb0-273">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="b2fb0-274">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="b2fb0-274">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="b2fb0-275">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-275">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="b2fb0-276"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-276"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="b2fb0-277">Ma ona jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-277">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="b2fb0-278">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-278">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="b2fb0-279">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-279">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="b2fb0-280"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-280"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="b2fb0-281"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-281">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="b2fb0-282">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-282">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="b2fb0-283">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-283">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="b2fb0-284"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-284"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="b2fb0-285">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-285">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="b2fb0-286"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-286"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="b2fb0-287">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-287">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="b2fb0-288">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-288">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="b2fb0-289">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="b2fb0-289">General options configuration</span></span>

<span data-ttu-id="b2fb0-290">Konfiguracja opcji ogólnych jest przedstawiona jako przykład 1 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-290">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="b2fb0-291">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-291">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b2fb0-292">Następująca Klasa `MyOptions`ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-292">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b2fb0-293">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-293">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b2fb0-294">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-294">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b2fb0-295">Klasa `MyOptions` zostanie dodana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-295">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b2fb0-296">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-296">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b2fb0-297">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-297">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="b2fb0-298">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-298">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="b2fb0-299">W przypadku używania <xref:System.Configuration.ConfigurationBuilder> niestandardowego do załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-299">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="b2fb0-300">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-300">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b2fb0-301">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="b2fb0-301">Configure simple options with a delegate</span></span>

<span data-ttu-id="b2fb0-302">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład 2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-302">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="b2fb0-303">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-303">Use a delegate to set options values.</span></span> <span data-ttu-id="b2fb0-304">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-304">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b2fb0-305">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-305">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b2fb0-306">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-306">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b2fb0-307">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-307">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b2fb0-308">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-308">You can add multiple configuration providers.</span></span> <span data-ttu-id="b2fb0-309">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-309">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="b2fb0-310">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-310">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="b2fb0-311">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-311">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="b2fb0-312">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są przesłonięte przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-312">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b2fb0-313">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-313">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b2fb0-314">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-314">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b2fb0-315">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-315">Suboptions configuration</span></span>

<span data-ttu-id="b2fb0-316">Konfiguracja podopcji jest przedstawiana jako przykład 3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-316">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="b2fb0-317">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-317">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="b2fb0-318">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-318">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b2fb0-319">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-319">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b2fb0-320">Na przykład właściwość `MyOptions.Option1` jest powiązana z `Option1`klucza, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-320">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b2fb0-321">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-321">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b2fb0-322">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b2fb0-322">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b2fb0-323">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-323">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="b2fb0-324">Plik *appSettings. JSON* przykładu definiuje `subsection` składową z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-324">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b2fb0-325">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`do przechowywania wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-325">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b2fb0-326">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-326">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b2fb0-327">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-327">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="b2fb0-328">Iniekcja opcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-328">Options injection</span></span>

<span data-ttu-id="b2fb0-329">Iniekcja opcji jest przedstawiana jako przykład 4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-329">Options injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="b2fb0-330">Wsuń <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-330">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="b2fb0-331">Strona Razor lub widok MVC z dyrektywą [`@inject`](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-331">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="b2fb0-332">Model strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-332">A page or view model.</span></span>

<span data-ttu-id="b2fb0-333">Poniższy przykład z przykładowej aplikacji wprowadza <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do modelu strony (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-333">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b2fb0-334">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` za pomocą dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-334">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="b2fb0-335">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-335">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b2fb0-337">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b2fb0-337">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b2fb0-338">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie 5 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-338">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="b2fb0-339">Przy użyciu <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>opcje są obliczane raz dla każdego żądania, gdy jest on używany i buforowany przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-339">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="b2fb0-340">Różnica między `IOptionsMonitor` i `IOptionsSnapshot` to:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-340">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="b2fb0-341">`IOptionsMonitor` to [Pojedyncza usługa](xref:fundamentals/dependency-injection#singleton) , która pobiera bieżące wartości opcji w dowolnym momencie, która jest szczególnie przydatna w pojedynczych zależnościach.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-341">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="b2fb0-342">`IOptionsSnapshot` jest [usługą objętą zakresem](xref:fundamentals/dependency-injection#scoped) i zawiera migawkę opcji w czasie konstruowania obiektu `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-342">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="b2fb0-343">Migawki opcji są przeznaczone do użycia z zależnościami przejściowymi i zakresowymi.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-343">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="b2fb0-344">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-344">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b2fb0-345">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-345">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b2fb0-346">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b2fb0-346">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b2fb0-347">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-347">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b2fb0-348">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-348">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b2fb0-349">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-349">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b2fb0-350">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="b2fb0-350">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b2fb0-351">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład 6 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-351">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="b2fb0-352">Obsługa "nazwanych opcji" pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-352">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b2fb0-353">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Skonfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-353">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="b2fb0-354">W nazwanych opcjach jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-354">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b2fb0-355">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-355">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b2fb0-356">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-356">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b2fb0-357">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-357">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b2fb0-358">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-358">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b2fb0-359">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-359">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b2fb0-360">Wartość domyślna dla `Option2` dostarczonej przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-360">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="b2fb0-361">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="b2fb0-361">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="b2fb0-362">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-362">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="b2fb0-363">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-363">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="b2fb0-364">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-364">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b2fb0-365">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-365">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b2fb0-366">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-366">All options are named instances.</span></span> <span data-ttu-id="b2fb0-367">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-367">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b2fb0-368"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-368"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="b2fb0-369">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-369">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="b2fb0-370">Opcja `null` nazwana służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-370">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="b2fb0-371">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="b2fb0-371">OptionsBuilder API</span></span>

<span data-ttu-id="b2fb0-372"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-372"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="b2fb0-373">`OptionsBuilder` usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego wywołania `AddOptions<TOptions>(string optionsName)`, a nie pojawiają się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-373">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="b2fb0-374">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-374">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="b2fb0-375">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-375">Use DI services to configure options</span></span>

<span data-ttu-id="b2fb0-376">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-376">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="b2fb0-377">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-377">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="b2fb0-378">Program [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) oferuje przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które umożliwiają konfigurowanie opcji przy użyciu maksymalnie pięciu usług:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-378">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="b2fb0-379">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-379">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="b2fb0-380">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-380">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="b2fb0-381">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-381">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="b2fb0-382">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-382">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="b2fb0-383">Sprawdzanie poprawności opcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-383">Options validation</span></span>

<span data-ttu-id="b2fb0-384">Sprawdzanie poprawności opcji umożliwia sprawdzenie opcji w przypadku skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-384">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="b2fb0-385">Wywołaj `Validate` z metodą walidacji, która zwraca `true`, jeśli opcje są prawidłowe i `false` jeśli są nieprawidłowe:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-385">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="b2fb0-386">Poprzedni przykład ustawia nazwane wystąpienie opcji na `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-386">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="b2fb0-387">Domyślne wystąpienie opcji jest `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-387">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="b2fb0-388">Walidacja jest uruchamiana po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-388">Validation runs when the options instance is created.</span></span> <span data-ttu-id="b2fb0-389">Wystąpienie opcji gwarantuje przekazanie walidacji przy pierwszym dostępie do niego.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-389">An options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b2fb0-390">Sprawdzanie poprawności opcji nie chroni przed modyfikacjami opcji po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-390">Options validation doesn't guard against options modifications after the options instance is created.</span></span> <span data-ttu-id="b2fb0-391">Na przykład opcje `IOptionsSnapshot` są tworzone i weryfikowane raz na żądanie po pierwszym uzyskaniu dostępu do tych opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-391">For example, `IOptionsSnapshot` options are created and validated once per request when the options are first accessed.</span></span> <span data-ttu-id="b2fb0-392">Opcje `IOptionsSnapshot` nie są weryfikowane ponownie przy kolejnych próbach dostępu *dla tego samego żądania*.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-392">The `IOptionsSnapshot` options aren't validated again on subsequent access attempts *for the same request*.</span></span>

<span data-ttu-id="b2fb0-393">Metoda `Validate` akceptuje `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-393">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="b2fb0-394">Aby w pełni dostosować walidację, zaimplementuj `IValidateOptions<TOptions>`, co umożliwia:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-394">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="b2fb0-395">Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="b2fb0-395">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="b2fb0-396">Walidacja zależna od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="b2fb0-396">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="b2fb0-397">`IValidateOptions` sprawdza poprawność:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-397">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="b2fb0-398">Konkretne nazwane wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-398">A specific named options instance.</span></span>
* <span data-ttu-id="b2fb0-399">Wszystkie opcje, gdy `name` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-399">All options when `name` is `null`.</span></span>

<span data-ttu-id="b2fb0-400">Zwróć `ValidateOptionsResult` z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-400">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="b2fb0-401">Weryfikacja oparta na adnotacji danych jest dostępna w pakiecie [Microsoft. Extensions. options. DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) , wywołując metodę <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> w `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-401">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="b2fb0-402">`Microsoft.Extensions.Options.DataAnnotations` jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-402">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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

<span data-ttu-id="b2fb0-403">Eager sprawdzanie poprawności (niepowodzenie szybkie przy uruchamianiu) jest rozważane dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-403">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="b2fb0-404">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-404">Options post-configuration</span></span>

<span data-ttu-id="b2fb0-405">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-405">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="b2fb0-406">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-406">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b2fb0-407"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępny do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-407"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b2fb0-408">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-408">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="b2fb0-409">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b2fb0-409">Accessing options during startup</span></span>

<span data-ttu-id="b2fb0-410"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-410"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="b2fb0-411">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-411">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b2fb0-412">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-412">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="b2fb0-413">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-413">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b2fb0-414">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-414">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b2fb0-415">[Zasady podziału interfejsu (ISP) lub &ndash; hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-415">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b2fb0-416">[Separacja problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień różnych części aplikacji nie zależy od siebie ani nie jest połączona.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-416">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b2fb0-417">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-417">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="b2fb0-418">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-418">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="b2fb0-419">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b2fb0-419">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b2fb0-420">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b2fb0-420">Prerequisites</span></span>

<span data-ttu-id="b2fb0-421">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-421">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="b2fb0-422">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-422">Options interfaces</span></span>

<span data-ttu-id="b2fb0-423"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-423"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="b2fb0-424"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-424"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="b2fb0-425">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="b2fb0-425">Change notifications</span></span>
* [<span data-ttu-id="b2fb0-426">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="b2fb0-426">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="b2fb0-427">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="b2fb0-427">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="b2fb0-428">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="b2fb0-428">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="b2fb0-429">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-429">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="b2fb0-430"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-430"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="b2fb0-431">Ma ona jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-431">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="b2fb0-432">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-432">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="b2fb0-433">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-433">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="b2fb0-434"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-434"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="b2fb0-435"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-435">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="b2fb0-436">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-436">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="b2fb0-437">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-437">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="b2fb0-438"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-438"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="b2fb0-439">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-439">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="b2fb0-440"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-440"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="b2fb0-441">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-441">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="b2fb0-442">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-442">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="b2fb0-443">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="b2fb0-443">General options configuration</span></span>

<span data-ttu-id="b2fb0-444">Konfiguracja opcji ogólnych jest przedstawiona jako przykład 1 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-444">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="b2fb0-445">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-445">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b2fb0-446">Następująca Klasa `MyOptions`ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-446">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b2fb0-447">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-447">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b2fb0-448">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-448">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b2fb0-449">Klasa `MyOptions` zostanie dodana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-449">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b2fb0-450">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-450">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b2fb0-451">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-451">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="b2fb0-452">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-452">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="b2fb0-453">W przypadku używania <xref:System.Configuration.ConfigurationBuilder> niestandardowego do załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-453">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="b2fb0-454">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-454">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b2fb0-455">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="b2fb0-455">Configure simple options with a delegate</span></span>

<span data-ttu-id="b2fb0-456">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład 2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-456">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="b2fb0-457">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-457">Use a delegate to set options values.</span></span> <span data-ttu-id="b2fb0-458">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-458">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b2fb0-459">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-459">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b2fb0-460">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-460">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b2fb0-461">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-461">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b2fb0-462">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-462">You can add multiple configuration providers.</span></span> <span data-ttu-id="b2fb0-463">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-463">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="b2fb0-464">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-464">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="b2fb0-465">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-465">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="b2fb0-466">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są przesłonięte przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-466">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b2fb0-467">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-467">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b2fb0-468">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-468">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b2fb0-469">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-469">Suboptions configuration</span></span>

<span data-ttu-id="b2fb0-470">Konfiguracja podopcji jest przedstawiana jako przykład 3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-470">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="b2fb0-471">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-471">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="b2fb0-472">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-472">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b2fb0-473">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-473">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b2fb0-474">Na przykład właściwość `MyOptions.Option1` jest powiązana z `Option1`klucza, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-474">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b2fb0-475">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-475">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b2fb0-476">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b2fb0-476">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b2fb0-477">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-477">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="b2fb0-478">Plik *appSettings. JSON* przykładu definiuje `subsection` składową z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-478">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b2fb0-479">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`do przechowywania wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-479">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b2fb0-480">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-480">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b2fb0-481">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-481">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="b2fb0-482">Opcje udostępniane przez model widoku lub bezpośrednie iniekcja widoku</span><span class="sxs-lookup"><span data-stu-id="b2fb0-482">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="b2fb0-483">Opcje udostępniane przez model widoku lub bezpośrednie wstrzyknięcie widoku są przedstawiane jako przykład 4 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-483">Options provided by a view model or with direct view injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="b2fb0-484">Opcje można dostarczyć w modelu widoku lub poprzez wstrzyknięcie <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> bezpośrednio do widoku (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-484">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b2fb0-485">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` za pomocą dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-485">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="b2fb0-486">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-486">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b2fb0-488">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b2fb0-488">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b2fb0-489">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie 5 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-489">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="b2fb0-490"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> obsługuje opcje ponownego ładowania z minimalnym obciążeniem przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-490"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="b2fb0-491">Opcje są obliczane raz dla żądania w przypadku uzyskiwania dostępu do pamięci podręcznej i buforowania jej przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-491">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="b2fb0-492">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-492">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b2fb0-493">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-493">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b2fb0-494">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b2fb0-494">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b2fb0-495">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-495">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b2fb0-496">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-496">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b2fb0-497">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-497">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b2fb0-498">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="b2fb0-498">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b2fb0-499">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład 6 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-499">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="b2fb0-500">Obsługa "nazwanych opcji" pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-500">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b2fb0-501">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Skonfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-501">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="b2fb0-502">W nazwanych opcjach jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-502">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b2fb0-503">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b2fb0-503">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b2fb0-504">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-504">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b2fb0-505">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b2fb0-505">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b2fb0-506">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-506">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b2fb0-507">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-507">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b2fb0-508">Wartość domyślna dla `Option2` dostarczonej przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-508">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="b2fb0-509">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="b2fb0-509">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="b2fb0-510">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-510">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="b2fb0-511">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-511">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="b2fb0-512">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-512">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b2fb0-513">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-513">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b2fb0-514">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-514">All options are named instances.</span></span> <span data-ttu-id="b2fb0-515">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-515">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b2fb0-516"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-516"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="b2fb0-517">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-517">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="b2fb0-518">Opcja `null` nazwana służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-518">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="b2fb0-519">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="b2fb0-519">OptionsBuilder API</span></span>

<span data-ttu-id="b2fb0-520"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-520"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="b2fb0-521">`OptionsBuilder` usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego wywołania `AddOptions<TOptions>(string optionsName)`, a nie pojawiają się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-521">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="b2fb0-522">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-522">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="b2fb0-523">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-523">Use DI services to configure options</span></span>

<span data-ttu-id="b2fb0-524">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-524">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="b2fb0-525">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-525">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="b2fb0-526">Program [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) oferuje przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które umożliwiają konfigurowanie opcji przy użyciu maksymalnie pięciu usług:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-526">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="b2fb0-527">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-527">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="b2fb0-528">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-528">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="b2fb0-529">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="b2fb0-529">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="b2fb0-530">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-530">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="b2fb0-531">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="b2fb0-531">Options post-configuration</span></span>

<span data-ttu-id="b2fb0-532">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-532">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="b2fb0-533">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-533">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b2fb0-534"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępny do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-534"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b2fb0-535">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="b2fb0-535">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="b2fb0-536">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b2fb0-536">Accessing options during startup</span></span>

<span data-ttu-id="b2fb0-537"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-537"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="b2fb0-538">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-538">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b2fb0-539">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="b2fb0-539">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b2fb0-540">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b2fb0-540">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
