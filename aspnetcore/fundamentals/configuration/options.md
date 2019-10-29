---
title: Wzorzec opcji w ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać wzorca opcji do reprezentowania grup powiązanych ustawień w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/28/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: f9e94e8d1736b7ffaa2640aba03da6b239a34f0a
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034011"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="b457e-103">Wzorzec opcji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b457e-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="b457e-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b457e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b457e-105">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="b457e-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b457e-106">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="b457e-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b457e-107">[Zasada segregowania interfejsu (ISP) lub hermetyzacja](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="b457e-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b457e-108">[Rozdzielenie problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; dla różnych części aplikacji nie jest zależne ani powiązane ze sobą.</span><span class="sxs-lookup"><span data-stu-id="b457e-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b457e-109">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="b457e-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="b457e-110">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="b457e-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="b457e-111">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b457e-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="b457e-112">Package</span><span class="sxs-lookup"><span data-stu-id="b457e-112">Package</span></span>

<span data-ttu-id="b457e-113">Pakiet [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) jest niejawnie przywoływany w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b457e-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="b457e-114">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="b457e-114">Options interfaces</span></span>

<span data-ttu-id="b457e-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="b457e-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="b457e-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="b457e-117">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="b457e-117">Change notifications</span></span>
* [<span data-ttu-id="b457e-118">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="b457e-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="b457e-119">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="b457e-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="b457e-120">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="b457e-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="b457e-121">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="b457e-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="b457e-123">Ma jedną metodę <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="b457e-124">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="b457e-125">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="b457e-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="b457e-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="b457e-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="b457e-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="b457e-128">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="b457e-129">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="b457e-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="b457e-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b457e-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="b457e-131">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="b457e-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="b457e-132"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="b457e-133">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="b457e-134">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="b457e-135">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="b457e-135">General options configuration</span></span>

<span data-ttu-id="b457e-136">Konfiguracja opcji ogólnych jest przedstawiana jako przykład &num;1 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="b457e-137">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="b457e-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b457e-138">Następująca Klasa, `MyOptions`, ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b457e-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b457e-139">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b457e-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b457e-140">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b457e-141">Klasa `MyOptions` jest dodawana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="b457e-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b457e-142">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b457e-143">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="b457e-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="b457e-144">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="b457e-145">W przypadku używania niestandardowego <xref:System.Configuration.ConfigurationBuilder> w celu załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="b457e-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="b457e-146">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b457e-147">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="b457e-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="b457e-148">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład &num;2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="b457e-149">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-149">Use a delegate to set options values.</span></span> <span data-ttu-id="b457e-150">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b457e-151">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b457e-152">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b457e-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b457e-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b457e-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b457e-154">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="b457e-155">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="b457e-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="b457e-156">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b457e-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="b457e-157">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="b457e-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="b457e-158">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są zastępowane przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="b457e-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b457e-159">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b457e-160">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b457e-161">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="b457e-161">Suboptions configuration</span></span>

<span data-ttu-id="b457e-162">Konfiguracja podopcji jest przedstawiana jako przykład &num;3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="b457e-163">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="b457e-164">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="b457e-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b457e-165">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b457e-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b457e-166">Na przykład właściwość `MyOptions.Option1` jest powiązana z kluczem `Option1`, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b457e-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b457e-167">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b457e-168">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b457e-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b457e-169">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="b457e-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="b457e-170">Plik *appSettings. JSON* przykładu definiuje element członkowski `subsection` z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b457e-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b457e-171">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`, aby przechowywać wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b457e-172">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b457e-173">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="b457e-174">Opcje udostępniane przez model widoku lub bezpośrednie iniekcja widoku</span><span class="sxs-lookup"><span data-stu-id="b457e-174">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="b457e-175">Opcje udostępniane przez model widoku lub bezpośrednie wstrzyknięcie widoku są przedstawiane jako przykład &num;4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-175">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="b457e-176">Opcje można dostarczyć w modelu widoku lub poprzez wstrzyknięcie <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> bezpośrednio do widoku (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-176">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b457e-177">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` z dyrektywą `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b457e-177">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="b457e-178">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-178">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b457e-180">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b457e-180">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b457e-181">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie &num;5 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-181">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="b457e-182"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> obsługuje opcje ponownego ładowania z minimalnym obciążeniem przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="b457e-182"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="b457e-183">Opcje są obliczane raz dla żądania w przypadku uzyskiwania dostępu do pamięci podręcznej i buforowania jej przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="b457e-183">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="b457e-184">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="b457e-184">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b457e-185">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-185">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b457e-186">Na poniższej ilustracji przedstawiono początkowe wartości `option1` i `option2` załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b457e-186">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b457e-187">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="b457e-187">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b457e-188">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b457e-188">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b457e-189">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="b457e-189">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b457e-190">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="b457e-190">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b457e-191">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład &num;6 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-191">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="b457e-192">Obsługa " *nazwanych opcji* " pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-192">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b457e-193">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Konfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="b457e-193">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b457e-194">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-194">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b457e-195">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="b457e-195">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b457e-196">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b457e-196">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b457e-197">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="b457e-197">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b457e-198">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b457e-198">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b457e-199">Wartość domyślna dla `Option2` udostępniana przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-199">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="b457e-200">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="b457e-200">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="b457e-201">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-201">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="b457e-202">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="b457e-202">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="b457e-203">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b457e-203">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b457e-204">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="b457e-204">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b457e-205">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="b457e-205">All options are named instances.</span></span> <span data-ttu-id="b457e-206">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b457e-206">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b457e-207"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-207"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="b457e-208">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="b457e-208">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="b457e-209">Opcja o nazwie `null` służy do kierowania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> Użyj tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="b457e-209">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="b457e-210">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="b457e-210">OptionsBuilder API</span></span>

<span data-ttu-id="b457e-211"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-211"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="b457e-212">`OptionsBuilder` upraszcza tworzenie nazwanych opcji, ponieważ jest to tylko jeden parametr do początkowego wywołania `AddOptions<TOptions>(string optionsName)` zamiast pojawienia się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="b457e-212">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="b457e-213">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b457e-213">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="b457e-214">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="b457e-214">Use DI services to configure options</span></span>

<span data-ttu-id="b457e-215">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="b457e-215">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="b457e-216">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="b457e-216">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="b457e-217">Program [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) oferuje przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które umożliwiają konfigurowanie opcji przy użyciu maksymalnie pięciu usług:</span><span class="sxs-lookup"><span data-stu-id="b457e-217">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="b457e-218">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="b457e-218">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="b457e-219">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="b457e-219">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="b457e-220">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="b457e-220">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="b457e-221">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="b457e-221">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="b457e-222">Sprawdzanie poprawności opcji</span><span class="sxs-lookup"><span data-stu-id="b457e-222">Options validation</span></span>

<span data-ttu-id="b457e-223">Sprawdzanie poprawności opcji umożliwia sprawdzenie opcji w przypadku skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-223">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="b457e-224">Wywołaj `Validate` z metodą walidacji zwracającą `true`, jeśli opcje są prawidłowe i `false`, jeśli są nieprawidłowe:</span><span class="sxs-lookup"><span data-stu-id="b457e-224">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="b457e-225">Poprzedni przykład ustawia nazwane wystąpienie opcji na `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="b457e-225">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="b457e-226">Domyślne wystąpienie opcji to `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="b457e-226">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="b457e-227">Walidacja jest uruchamiana po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-227">Validation runs when the options instance is created.</span></span> <span data-ttu-id="b457e-228">Twoje wystąpienie opcji gwarantuje przekazanie walidacji przy pierwszej próbie dostępu.</span><span class="sxs-lookup"><span data-stu-id="b457e-228">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b457e-229">Sprawdzanie poprawności opcji nie chroni przed modyfikacjami opcji po początkowym skonfigurowaniu opcji i sprawdzeniu poprawności.</span><span class="sxs-lookup"><span data-stu-id="b457e-229">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="b457e-230">Metoda `Validate` akceptuje `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="b457e-230">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="b457e-231">Aby w pełni dostosować walidację, zaimplementuj `IValidateOptions<TOptions>`, co umożliwia:</span><span class="sxs-lookup"><span data-stu-id="b457e-231">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="b457e-232">Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="b457e-232">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="b457e-233">Walidacja zależna od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="b457e-233">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="b457e-234">`IValidateOptions` sprawdza poprawność:</span><span class="sxs-lookup"><span data-stu-id="b457e-234">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="b457e-235">Konkretne nazwane wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-235">A specific named options instance.</span></span>
* <span data-ttu-id="b457e-236">Wszystkie opcje, gdy `name` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="b457e-236">All options when `name` is `null`.</span></span>

<span data-ttu-id="b457e-237">Zwróć `ValidateOptionsResult` z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="b457e-237">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="b457e-238">Weryfikacja oparta na adnotacji danych jest dostępna w pakiecie [Microsoft. Extensions. options. DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) , wywołując metodę <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> na `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="b457e-238">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="b457e-239">`Microsoft.Extensions.Options.DataAnnotations` jest niejawnie przywoływany w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b457e-239">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

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

<span data-ttu-id="b457e-240">Eager sprawdzanie poprawności (niepowodzenie szybkie przy uruchamianiu) jest rozważane dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="b457e-240">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="b457e-241">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="b457e-241">Options post-configuration</span></span>

<span data-ttu-id="b457e-242">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-242">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="b457e-243">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="b457e-243">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b457e-244"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępna do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-244"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b457e-245">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="b457e-245">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="b457e-246">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b457e-246">Accessing options during startup</span></span>

<span data-ttu-id="b457e-247"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b457e-247"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="b457e-248">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b457e-248">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b457e-249">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="b457e-249">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="b457e-250">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="b457e-250">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b457e-251">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="b457e-251">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b457e-252">[Zasada segregowania interfejsu (ISP) lub hermetyzacja](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="b457e-252">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b457e-253">[Rozdzielenie problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; dla różnych części aplikacji nie jest zależne ani powiązane ze sobą.</span><span class="sxs-lookup"><span data-stu-id="b457e-253">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b457e-254">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="b457e-254">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="b457e-255">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="b457e-255">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="b457e-256">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b457e-256">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b457e-257">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b457e-257">Prerequisites</span></span>

<span data-ttu-id="b457e-258">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="b457e-258">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="b457e-259">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="b457e-259">Options interfaces</span></span>

<span data-ttu-id="b457e-260"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-260"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="b457e-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="b457e-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="b457e-262">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="b457e-262">Change notifications</span></span>
* [<span data-ttu-id="b457e-263">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="b457e-263">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="b457e-264">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="b457e-264">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="b457e-265">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="b457e-265">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="b457e-266">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-266">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="b457e-267"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-267"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="b457e-268">Ma jedną metodę <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-268">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="b457e-269">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-269">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="b457e-270">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="b457e-270">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="b457e-271"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-271"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="b457e-272"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="b457e-272">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="b457e-273">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-273">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="b457e-274">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="b457e-274">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="b457e-275"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b457e-275"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="b457e-276">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="b457e-276">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="b457e-277"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-277"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="b457e-278">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-278">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="b457e-279">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-279">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="b457e-280">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="b457e-280">General options configuration</span></span>

<span data-ttu-id="b457e-281">Konfiguracja opcji ogólnych jest przedstawiana jako przykład &num;1 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-281">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="b457e-282">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="b457e-282">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b457e-283">Następująca Klasa, `MyOptions`, ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b457e-283">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b457e-284">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b457e-284">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b457e-285">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-285">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b457e-286">Klasa `MyOptions` jest dodawana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="b457e-286">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b457e-287">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-287">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b457e-288">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="b457e-288">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="b457e-289">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-289">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="b457e-290">W przypadku używania niestandardowego <xref:System.Configuration.ConfigurationBuilder> w celu załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="b457e-290">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="b457e-291">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-291">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b457e-292">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="b457e-292">Configure simple options with a delegate</span></span>

<span data-ttu-id="b457e-293">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład &num;2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-293">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="b457e-294">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-294">Use a delegate to set options values.</span></span> <span data-ttu-id="b457e-295">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-295">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b457e-296">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-296">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b457e-297">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b457e-297">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b457e-298">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b457e-298">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b457e-299">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-299">You can add multiple configuration providers.</span></span> <span data-ttu-id="b457e-300">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="b457e-300">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="b457e-301">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b457e-301">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="b457e-302">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="b457e-302">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="b457e-303">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są zastępowane przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="b457e-303">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b457e-304">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-304">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b457e-305">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-305">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b457e-306">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="b457e-306">Suboptions configuration</span></span>

<span data-ttu-id="b457e-307">Konfiguracja podopcji jest przedstawiana jako przykład &num;3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-307">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="b457e-308">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-308">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="b457e-309">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="b457e-309">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b457e-310">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b457e-310">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b457e-311">Na przykład właściwość `MyOptions.Option1` jest powiązana z kluczem `Option1`, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b457e-311">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b457e-312">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-312">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b457e-313">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b457e-313">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b457e-314">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="b457e-314">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="b457e-315">Plik *appSettings. JSON* przykładu definiuje element członkowski `subsection` z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b457e-315">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b457e-316">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`, aby przechowywać wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-316">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b457e-317">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-317">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b457e-318">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-318">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="b457e-319">Opcje udostępniane przez model widoku lub bezpośrednie iniekcja widoku</span><span class="sxs-lookup"><span data-stu-id="b457e-319">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="b457e-320">Opcje udostępniane przez model widoku lub bezpośrednie wstrzyknięcie widoku są przedstawiane jako przykład &num;4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-320">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="b457e-321">Opcje można dostarczyć w modelu widoku lub poprzez wstrzyknięcie <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> bezpośrednio do widoku (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-321">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b457e-322">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` z dyrektywą `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b457e-322">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="b457e-323">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-323">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b457e-325">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b457e-325">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b457e-326">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie &num;5 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-326">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="b457e-327"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> obsługuje opcje ponownego ładowania z minimalnym obciążeniem przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="b457e-327"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="b457e-328">Opcje są obliczane raz dla żądania w przypadku uzyskiwania dostępu do pamięci podręcznej i buforowania jej przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="b457e-328">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="b457e-329">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="b457e-329">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b457e-330">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-330">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b457e-331">Na poniższej ilustracji przedstawiono początkowe wartości `option1` i `option2` załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b457e-331">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b457e-332">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="b457e-332">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b457e-333">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b457e-333">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b457e-334">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="b457e-334">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b457e-335">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="b457e-335">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b457e-336">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład &num;6 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-336">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="b457e-337">Obsługa " *nazwanych opcji* " pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-337">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b457e-338">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Konfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="b457e-338">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b457e-339">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-339">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b457e-340">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="b457e-340">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b457e-341">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b457e-341">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b457e-342">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="b457e-342">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b457e-343">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b457e-343">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b457e-344">Wartość domyślna dla `Option2` udostępniana przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-344">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="b457e-345">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="b457e-345">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="b457e-346">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-346">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="b457e-347">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="b457e-347">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="b457e-348">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b457e-348">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b457e-349">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="b457e-349">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b457e-350">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="b457e-350">All options are named instances.</span></span> <span data-ttu-id="b457e-351">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b457e-351">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b457e-352"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-352"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="b457e-353">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="b457e-353">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="b457e-354">Opcja o nazwie `null` służy do kierowania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> Użyj tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="b457e-354">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="b457e-355">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="b457e-355">OptionsBuilder API</span></span>

<span data-ttu-id="b457e-356"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-356"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="b457e-357">`OptionsBuilder` upraszcza tworzenie nazwanych opcji, ponieważ jest to tylko jeden parametr do początkowego wywołania `AddOptions<TOptions>(string optionsName)` zamiast pojawienia się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="b457e-357">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="b457e-358">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b457e-358">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="b457e-359">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="b457e-359">Use DI services to configure options</span></span>

<span data-ttu-id="b457e-360">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="b457e-360">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="b457e-361">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="b457e-361">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="b457e-362">Program [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) oferuje przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które umożliwiają konfigurowanie opcji przy użyciu maksymalnie pięciu usług:</span><span class="sxs-lookup"><span data-stu-id="b457e-362">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="b457e-363">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="b457e-363">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="b457e-364">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="b457e-364">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="b457e-365">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="b457e-365">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="b457e-366">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="b457e-366">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="b457e-367">Sprawdzanie poprawności opcji</span><span class="sxs-lookup"><span data-stu-id="b457e-367">Options validation</span></span>

<span data-ttu-id="b457e-368">Sprawdzanie poprawności opcji umożliwia sprawdzenie opcji w przypadku skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-368">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="b457e-369">Wywołaj `Validate` z metodą walidacji zwracającą `true`, jeśli opcje są prawidłowe i `false`, jeśli są nieprawidłowe:</span><span class="sxs-lookup"><span data-stu-id="b457e-369">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="b457e-370">Poprzedni przykład ustawia nazwane wystąpienie opcji na `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="b457e-370">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="b457e-371">Domyślne wystąpienie opcji to `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="b457e-371">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="b457e-372">Walidacja jest uruchamiana po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-372">Validation runs when the options instance is created.</span></span> <span data-ttu-id="b457e-373">Twoje wystąpienie opcji gwarantuje przekazanie walidacji przy pierwszej próbie dostępu.</span><span class="sxs-lookup"><span data-stu-id="b457e-373">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b457e-374">Sprawdzanie poprawności opcji nie chroni przed modyfikacjami opcji po początkowym skonfigurowaniu opcji i sprawdzeniu poprawności.</span><span class="sxs-lookup"><span data-stu-id="b457e-374">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="b457e-375">Metoda `Validate` akceptuje `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="b457e-375">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="b457e-376">Aby w pełni dostosować walidację, zaimplementuj `IValidateOptions<TOptions>`, co umożliwia:</span><span class="sxs-lookup"><span data-stu-id="b457e-376">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="b457e-377">Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="b457e-377">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="b457e-378">Walidacja zależna od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="b457e-378">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="b457e-379">`IValidateOptions` sprawdza poprawność:</span><span class="sxs-lookup"><span data-stu-id="b457e-379">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="b457e-380">Konkretne nazwane wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-380">A specific named options instance.</span></span>
* <span data-ttu-id="b457e-381">Wszystkie opcje, gdy `name` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="b457e-381">All options when `name` is `null`.</span></span>

<span data-ttu-id="b457e-382">Zwróć `ValidateOptionsResult` z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="b457e-382">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="b457e-383">Weryfikacja oparta na adnotacji danych jest dostępna w pakiecie [Microsoft. Extensions. options. DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) , wywołując metodę <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> na `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="b457e-383">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="b457e-384">`Microsoft.Extensions.Options.DataAnnotations` jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b457e-384">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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

<span data-ttu-id="b457e-385">Eager sprawdzanie poprawności (niepowodzenie szybkie przy uruchamianiu) jest rozważane dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="b457e-385">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="b457e-386">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="b457e-386">Options post-configuration</span></span>

<span data-ttu-id="b457e-387">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-387">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="b457e-388">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="b457e-388">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b457e-389"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępna do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-389"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b457e-390">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="b457e-390">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="b457e-391">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b457e-391">Accessing options during startup</span></span>

<span data-ttu-id="b457e-392"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b457e-392"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="b457e-393">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b457e-393">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b457e-394">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="b457e-394">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="b457e-395">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="b457e-395">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b457e-396">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="b457e-396">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b457e-397">[Zasada segregowania interfejsu (ISP) lub hermetyzacja](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="b457e-397">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b457e-398">[Rozdzielenie problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; dla różnych części aplikacji nie jest zależne ani powiązane ze sobą.</span><span class="sxs-lookup"><span data-stu-id="b457e-398">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b457e-399">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="b457e-399">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="b457e-400">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="b457e-400">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="b457e-401">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b457e-401">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b457e-402">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b457e-402">Prerequisites</span></span>

<span data-ttu-id="b457e-403">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="b457e-403">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="b457e-404">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="b457e-404">Options interfaces</span></span>

<span data-ttu-id="b457e-405"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-405"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="b457e-406"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="b457e-406"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="b457e-407">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="b457e-407">Change notifications</span></span>
* [<span data-ttu-id="b457e-408">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="b457e-408">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="b457e-409">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="b457e-409">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="b457e-410">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="b457e-410">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="b457e-411">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-411">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="b457e-412"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-412"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="b457e-413">Ma jedną metodę <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-413">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="b457e-414">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-414">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="b457e-415">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="b457e-415">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="b457e-416"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-416"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="b457e-417"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="b457e-417">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="b457e-418">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-418">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="b457e-419">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="b457e-419">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="b457e-420"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b457e-420"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="b457e-421">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="b457e-421">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="b457e-422"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-422"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="b457e-423">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-423">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="b457e-424">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-424">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="b457e-425">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="b457e-425">General options configuration</span></span>

<span data-ttu-id="b457e-426">Konfiguracja opcji ogólnych jest przedstawiana jako przykład &num;1 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-426">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="b457e-427">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="b457e-427">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b457e-428">Następująca Klasa, `MyOptions`, ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b457e-428">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b457e-429">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b457e-429">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b457e-430">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-430">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b457e-431">Klasa `MyOptions` jest dodawana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="b457e-431">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b457e-432">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-432">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b457e-433">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="b457e-433">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="b457e-434">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-434">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="b457e-435">W przypadku używania niestandardowego <xref:System.Configuration.ConfigurationBuilder> w celu załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="b457e-435">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="b457e-436">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-436">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b457e-437">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="b457e-437">Configure simple options with a delegate</span></span>

<span data-ttu-id="b457e-438">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład &num;2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-438">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="b457e-439">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-439">Use a delegate to set options values.</span></span> <span data-ttu-id="b457e-440">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-440">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b457e-441">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-441">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b457e-442">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b457e-442">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b457e-443">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b457e-443">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b457e-444">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-444">You can add multiple configuration providers.</span></span> <span data-ttu-id="b457e-445">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="b457e-445">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="b457e-446">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b457e-446">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="b457e-447">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="b457e-447">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="b457e-448">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są zastępowane przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="b457e-448">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b457e-449">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-449">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b457e-450">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-450">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b457e-451">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="b457e-451">Suboptions configuration</span></span>

<span data-ttu-id="b457e-452">Konfiguracja podopcji jest przedstawiana jako przykład &num;3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-452">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="b457e-453">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-453">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="b457e-454">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="b457e-454">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b457e-455">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b457e-455">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b457e-456">Na przykład właściwość `MyOptions.Option1` jest powiązana z kluczem `Option1`, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b457e-456">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b457e-457">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-457">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b457e-458">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b457e-458">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b457e-459">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="b457e-459">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="b457e-460">Plik *appSettings. JSON* przykładu definiuje element członkowski `subsection` z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b457e-460">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b457e-461">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`, aby przechowywać wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-461">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b457e-462">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-462">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b457e-463">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-463">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="b457e-464">Opcje udostępniane przez model widoku lub bezpośrednie iniekcja widoku</span><span class="sxs-lookup"><span data-stu-id="b457e-464">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="b457e-465">Opcje udostępniane przez model widoku lub bezpośrednie wstrzyknięcie widoku są przedstawiane jako przykład &num;4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-465">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="b457e-466">Opcje można dostarczyć w modelu widoku lub poprzez wstrzyknięcie <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> bezpośrednio do widoku (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-466">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b457e-467">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` z dyrektywą `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b457e-467">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="b457e-468">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-468">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b457e-470">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b457e-470">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b457e-471">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie &num;5 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-471">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="b457e-472"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> obsługuje opcje ponownego ładowania z minimalnym obciążeniem przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="b457e-472"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="b457e-473">Opcje są obliczane raz dla żądania w przypadku uzyskiwania dostępu do pamięci podręcznej i buforowania jej przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="b457e-473">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="b457e-474">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="b457e-474">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b457e-475">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b457e-475">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b457e-476">Na poniższej ilustracji przedstawiono początkowe wartości `option1` i `option2` załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="b457e-476">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b457e-477">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="b457e-477">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b457e-478">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b457e-478">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b457e-479">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="b457e-479">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b457e-480">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="b457e-480">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b457e-481">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład &num;6 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b457e-481">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="b457e-482">Obsługa " *nazwanych opcji* " pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="b457e-482">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b457e-483">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Konfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="b457e-483">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b457e-484">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="b457e-484">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b457e-485">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="b457e-485">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b457e-486">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="b457e-486">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b457e-487">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="b457e-487">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b457e-488">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b457e-488">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b457e-489">Wartość domyślna dla `Option2` udostępniana przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-489">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="b457e-490">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="b457e-490">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="b457e-491">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="b457e-491">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="b457e-492">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="b457e-492">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="b457e-493">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b457e-493">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b457e-494">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="b457e-494">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b457e-495">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="b457e-495">All options are named instances.</span></span> <span data-ttu-id="b457e-496">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b457e-496">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b457e-497"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-497"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="b457e-498">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="b457e-498">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="b457e-499">Opcja o nazwie `null` służy do kierowania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> Użyj tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="b457e-499">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="b457e-500">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="b457e-500">OptionsBuilder API</span></span>

<span data-ttu-id="b457e-501"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b457e-501"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="b457e-502">`OptionsBuilder` upraszcza tworzenie nazwanych opcji, ponieważ jest to tylko jeden parametr do początkowego wywołania `AddOptions<TOptions>(string optionsName)` zamiast pojawienia się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="b457e-502">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="b457e-503">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b457e-503">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="b457e-504">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="b457e-504">Use DI services to configure options</span></span>

<span data-ttu-id="b457e-505">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="b457e-505">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="b457e-506">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="b457e-506">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="b457e-507">Program [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) oferuje przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które umożliwiają konfigurowanie opcji przy użyciu maksymalnie pięciu usług:</span><span class="sxs-lookup"><span data-stu-id="b457e-507">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="b457e-508">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="b457e-508">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="b457e-509">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="b457e-509">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="b457e-510">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="b457e-510">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="b457e-511">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="b457e-511">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="b457e-512">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="b457e-512">Options post-configuration</span></span>

<span data-ttu-id="b457e-513">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b457e-513">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="b457e-514">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="b457e-514">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b457e-515"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępna do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="b457e-515"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b457e-516">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="b457e-516">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="b457e-517">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b457e-517">Accessing options during startup</span></span>

<span data-ttu-id="b457e-518"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b457e-518"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="b457e-519">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b457e-519">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b457e-520">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="b457e-520">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b457e-521">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b457e-521">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
