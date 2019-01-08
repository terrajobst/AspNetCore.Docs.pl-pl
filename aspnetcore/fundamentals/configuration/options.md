---
title: Wzorzec opcje w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak użyć wzorca opcje do reprezentowania grup powiązanych ustawień w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/29/2018
uid: fundamentals/configuration/options
ms.openlocfilehash: 20365a078327d76693a40fa79a4a594e29e0901c
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099250"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="9fcc8-103">Wzorzec opcje w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9fcc8-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="9fcc8-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9fcc8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="9fcc8-105">Dla wersji 1.1 w tym temacie, Pobierz [wzorzec opcje w programie ASP.NET Core (w wersji 1.1, plików PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="9fcc8-105">For the 1.1 version of this topic, download [Options pattern in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="9fcc8-106">Wzorzec opcje używa klas do reprezentowania grup powiązane ustawienia.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-106">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="9fcc8-107">Gdy [ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klas, aplikacja działa zgodnie z dwóch zasad ważne inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-107">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="9fcc8-108">[Zasady podziału interfejsu (ISP) lub hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; scenariuszy (klasy), które są zależne od ustawień konfiguracji tylko zależą od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-108">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="9fcc8-109">[Separacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień dla różnych części aplikacji nie są zależnych lub powiązanych ze sobą.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-109">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="9fcc8-110">Opcje również udostępniać mechanizm do sprawdzania poprawności danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-110">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="9fcc8-111">Aby uzyskać więcej informacji, zobacz [opcje weryfikacji](#options-validation) sekcji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-111">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="9fcc8-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9fcc8-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9fcc8-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9fcc8-113">Prerequisites</span></span>

<span data-ttu-id="9fcc8-114">Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-114">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="9fcc8-115">Opcje interfejsów</span><span class="sxs-lookup"><span data-stu-id="9fcc8-115">Options interfaces</span></span>

<span data-ttu-id="9fcc8-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> Służy do pobierania opcji i zarządzanie opcje powiadomienia dotyczące `TOptions` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="9fcc8-117"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-117"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> supports the following scenarios:</span></span>

* <span data-ttu-id="9fcc8-118">Powiadomienia o zmianach</span><span class="sxs-lookup"><span data-stu-id="9fcc8-118">Change notifications</span></span>
* [<span data-ttu-id="9fcc8-119">Opcje nazwane</span><span class="sxs-lookup"><span data-stu-id="9fcc8-119">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="9fcc8-120">Ładowany ponownie konfigurację</span><span class="sxs-lookup"><span data-stu-id="9fcc8-120">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="9fcc8-121">Opcje selektywnego unieważniania (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>)</span><span class="sxs-lookup"><span data-stu-id="9fcc8-121">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>)</span></span>

<span data-ttu-id="9fcc8-122">[Zadania po konfiguracji](#options-post-configuration) scenariuszach pozwala na ustawianie lub zmienianie po wszystkich opcji <xref:Microsoft.Extensions.Options.IConfigureOptions`1> występuje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-122">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions`1> configuration occurs.</span></span>

<span data-ttu-id="9fcc8-123"><xref:Microsoft.Extensions.Options.IOptionsFactory`1> jest odpowiedzialny za tworzenie nowych wystąpień opcje.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-123"><xref:Microsoft.Extensions.Options.IOptionsFactory`1> is responsible for creating new options instances.</span></span> <span data-ttu-id="9fcc8-124">Ma on jeden <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metody.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-124">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="9fcc8-125">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions`1> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1> i uruchamia wszystkie konfiguracje najpierw następuje po konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-125">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions`1> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="9fcc8-126">Jego rozróżnia <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> i <xref:Microsoft.Extensions.Options.IConfigureOptions`1> i tylko wywołuje odpowiedniego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-126">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> and <xref:Microsoft.Extensions.Options.IConfigureOptions`1> and only calls the appropriate interface.</span></span>

<span data-ttu-id="9fcc8-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> jest używany przez <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> do pamięci podręcznej `TOptions` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> to cache `TOptions` instances.</span></span> <span data-ttu-id="9fcc8-128"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> Unieważnia opcje wystąpień w monitorze, tak aby wartość jest ponownie przeliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="9fcc8-128">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="9fcc8-129">Można ręcznie wprowadzić wartości z <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-129">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="9fcc8-130"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> Metoda jest używana, gdy wszystkie wystąpienia nazwanego, należy odtworzyć na żądanie.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-130">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="9fcc8-131"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> jest przydatne w scenariuszach, gdzie opcje powinny być obliczane ponownie na każde żądanie.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-131"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="9fcc8-132">Aby uzyskać więcej informacji, zobacz [ponowne załadowanie danych konfiguracji o IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) sekcji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-132">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="9fcc8-133"><xref:Microsoft.Extensions.Options.IOptions`1> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-133"><xref:Microsoft.Extensions.Options.IOptions`1> can be used to support options.</span></span> <span data-ttu-id="9fcc8-134">Jednak <xref:Microsoft.Extensions.Options.IOptions`1> nie obsługuje powyższych scenariuszy z <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-134">However, <xref:Microsoft.Extensions.Options.IOptions`1> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span></span> <span data-ttu-id="9fcc8-135">Możesz nadal używać <xref:Microsoft.Extensions.Options.IOptions`1> z istniejącymi strukturami i bibliotek, które już używają <xref:Microsoft.Extensions.Options.IOptions`1> interfejsu i nie wymagają scenariuszy, dostarczone przez <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-135">You may continue to use <xref:Microsoft.Extensions.Options.IOptions`1> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions`1> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="9fcc8-136">Ogólne opcje konfiguracji</span><span class="sxs-lookup"><span data-stu-id="9fcc8-136">General options configuration</span></span>

<span data-ttu-id="9fcc8-137">Opcje ogólne konfiguracja jest przedstawiona jako przykład &num;1 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-137">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="9fcc8-138">Klasa opcji musi być nieabstrakcyjnej przy użyciu publicznego konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-138">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="9fcc8-139">Następujące klasy `MyOptions`, ma dwie właściwości `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-139">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="9fcc8-140">Ustawianie wartości domyślnych jest opcjonalne, ale konstruktora klasy w poniższym przykładzie ustawiono wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-140">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="9fcc8-141">`Option2` ma wartość domyślną, ustawiane przez bezpośrednie inicjowanie właściwości (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="9fcc8-141">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="9fcc8-142">`MyOptions` Klasy zostanie dodany do kontenera usługi za pomocą <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązane z konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-142">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="9fcc8-143">Stronie używa modelu [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> można uzyskać dostęp do ustawień (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="9fcc8-143">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="9fcc8-144">Przykładowe *appsettings.json* plik Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-144">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="9fcc8-145">Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-145">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="9fcc8-146">Korzystając z niestandardowego <xref:System.Configuration.ConfigurationBuilder> można załadować konfiguracji opcji z pliku ustawień, upewnij się, że ścieżka podstawowa jest prawidłowo:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-146">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="9fcc8-147">Jawne ustawianie ścieżka podstawowa nie jest wymagana podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-147">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="9fcc8-148">Skonfiguruj opcje prostego za pomocą delegata</span><span class="sxs-lookup"><span data-stu-id="9fcc8-148">Configure simple options with a delegate</span></span>

<span data-ttu-id="9fcc8-149">Konfigurowanie opcji prostego z delegatem przedstawiono przykład &num;2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-149">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="9fcc8-150">Użycie delegowania do ustawiania wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-150">Use a delegate to set options values.</span></span> <span data-ttu-id="9fcc8-151">Ta aplikacja używa przykładowych `MyOptionsWithDelegateConfig` klasy (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="9fcc8-151">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="9fcc8-152">W poniższym kodzie sekundy <xref:Microsoft.Extensions.Options.IConfigureOptions`1> usługi zostanie dodany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-152">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service is added to the service container.</span></span> <span data-ttu-id="9fcc8-153">Używa ona delegata do konfigurowania wiązania za pomocą `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-153">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="9fcc8-154">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-154">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="9fcc8-155">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-155">You can add multiple configuration providers.</span></span> <span data-ttu-id="9fcc8-156">Dostawcy konfiguracji są dostępne z pakietów NuGet i są stosowane w kolejności, że zostanie zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-156">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="9fcc8-157">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-157">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="9fcc8-158">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> dodaje <xref:Microsoft.Extensions.Options.IConfigureOptions`1> usługi service container.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-158">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service to the service container.</span></span> <span data-ttu-id="9fcc8-159">W powyższym przykładzie wartości `Option1` i `Option2` określono zarówno w *appsettings.json*, ale wartości `Option1` i `Option2` są zastępowane przez delegata skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-159">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="9fcc8-160">Po włączeniu więcej niż jednej konfiguracji usługi ostatni źródło konfiguracji określone *wins* i ustawia wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-160">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="9fcc8-161">Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-161">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="9fcc8-162">Konfiguracja suboptions</span><span class="sxs-lookup"><span data-stu-id="9fcc8-162">Suboptions configuration</span></span>

<span data-ttu-id="9fcc8-163">Suboptions konfiguracja jest przedstawiona jako przykład &num;3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-163">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="9fcc8-164">Aplikacje powinny zostać utworzone opcje klasy, które odnoszą się do danego scenariusza grupy (klasy) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-164">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="9fcc8-165">Części aplikacji, które wymagają wartości konfiguracji powinien mieć tylko dostęp do wartości konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-165">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="9fcc8-166">Podczas tworzenia powiązania opcje konfiguracji, każdej właściwości w typie opcji jest powiązany z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-166">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="9fcc8-167">Na przykład `MyOptions.Option1` właściwość jest powiązana z klucza `Option1`, które są odczytywane z `option1` właściwość *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-167">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="9fcc8-168">W poniższym kodzie, a trzeci <xref:Microsoft.Extensions.Options.IConfigureOptions`1> usługi zostanie dodany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-168">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service is added to the service container.</span></span> <span data-ttu-id="9fcc8-169">Powiąże `MySubOptions` do sekcji `subsection` z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-169">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="9fcc8-170">`GetSection` — Metoda rozszerzenia wymaga [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-170">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="9fcc8-171">Jeśli aplikacja korzysta z [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 lub nowszej), pakiet jest automatycznie dołączane.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-171">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="9fcc8-172">Przykładowe *appsettings.json* plik definiuje `subsection` członka za pomocą kluczy `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-172">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="9fcc8-173">`MySubOptions` Klasy definiuje właściwości, `SubOption1` i `SubOption2`, aby przechowywać wartości opcji (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="9fcc8-173">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="9fcc8-174">Model strony `OnGet` metoda zwraca ciąg zawierający wartości opcji (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="9fcc8-174">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="9fcc8-175">Gdy aplikacja jest uruchamiana, `OnGet` metoda zwraca ciąg przedstawiający suboption wartości klasy:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-175">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="9fcc8-176">Opcje dostarczane przez model widoku lub przy użyciu iniekcji bezpośrednie widoku</span><span class="sxs-lookup"><span data-stu-id="9fcc8-176">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="9fcc8-177">Opcje dostarczane przez model widoku lub przy użyciu iniekcji bezpośrednie widoku przedstawiono przykład &num;4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-177">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="9fcc8-178">Opcje mogą być podawane w modelu widoku lub przez iniekcję <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> bezpośrednio w widoku (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="9fcc8-178">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="9fcc8-179">Przykładowa aplikacja pokazuje, jak wstawić `IOptionsMonitor<MyOptions>` z `@inject` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-179">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="9fcc8-180">Gdy aplikacja jest uruchamiana, wartości opcje są wyświetlane na renderowanej stronie:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-180">When the app is run, the options values are shown in the rendered page:</span></span>

![Opcje wartości opcja1: value1_from_json i opcja2: -1 są ładowane z modelu, jak również iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="9fcc8-182">Załaduj ponownie dane konfiguracyjne z IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="9fcc8-182">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="9fcc8-183">Ponowne załadowanie danych konfiguracji z <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> przedstawiono w przykładzie &num;5 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-183">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="9fcc8-184"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> Obsługa ponownego ładowania opcji z minimalnym przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-184"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="9fcc8-185">Opcje są obliczane raz na każde żądanie, gdy dostęp do pamięci podręcznej i okresu istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-185">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="9fcc8-186">W poniższym przykładzie pokazano, jak nowy <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> został utworzony po *appsettings.json* zmiany (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="9fcc8-186">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="9fcc8-187">Wiele żądań do serwera zwracają stałe wartości, dostarczone przez *appsettings.json* pliku do momentu zmiany pliku i ponowne załadowanie konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-187">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="9fcc8-188">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-188">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="9fcc8-189">Zmień wartości w *appsettings.json* plik `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-189">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="9fcc8-190">Zapisz *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-190">Save the *appsettings.json* file.</span></span> <span data-ttu-id="9fcc8-191">Odśwież przeglądarkę, aby zobaczyć, że zostaną zaktualizowane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-191">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="9fcc8-192">Opcje pomocy technicznej, za pomocą IConfigureNamedOptions nazwane</span><span class="sxs-lookup"><span data-stu-id="9fcc8-192">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="9fcc8-193">Opcje pomocy technicznej, o nazwie <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> przedstawiono przykład &num;6 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-193">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="9fcc8-194">*O nazwie opcje* pomocy technicznej zezwala aplikacji na rozróżnienie między nazwane opcje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-194">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="9fcc8-195">W przykładowej aplikacji o nazwie opcje są uznane za pomocą [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje metodę [ConfigureNamedOptions\<TOptions >. Konfigurowanie](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-195">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="9fcc8-196">Przykładowa aplikacja uzyskuje dostęp do opcji o nazwie za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="9fcc8-196">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="9fcc8-197">Uruchamianie przykładowej aplikacji, zwracane są nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-197">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="9fcc8-198">`named_options_1` wartości są dostarczane z konfiguracji, które są ładowane z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-198">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="9fcc8-199">`named_options_2` wartości są dostarczane przez:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-199">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="9fcc8-200">`named_options_2` Delegowanie w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-200">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="9fcc8-201">Wartością domyślną dla `Option2` dostarczone przez `MyOptions` klasy.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-201">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="9fcc8-202">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="9fcc8-202">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="9fcc8-203">Konfigurowanie wszystkich wystąpień opcje <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> metody.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-203">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="9fcc8-204">Poniższy kod służy do konfigurowania `Option1` dla wszystkich wystąpień konfiguracji za pomocą wspólnej wartości.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-204">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="9fcc8-205">Dodaj następujący kod ręcznie do `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-205">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="9fcc8-206">Uruchamianie przykładowej aplikacji po dodaniu kod generuje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-206">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="9fcc8-207">Wszystkie opcje są nazwane wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-207">All options are named instances.</span></span> <span data-ttu-id="9fcc8-208">Istniejące <xref:Microsoft.Extensions.Options.IConfigureOptions`1> wystąpienia są traktowane jako docelowy `Options.DefaultName` wystąpienia, co jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-208">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions`1> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="9fcc8-209"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-209"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span></span> <span data-ttu-id="9fcc8-210">Domyślna implementacja klasy <xref:Microsoft.Extensions.Options.IOptionsFactory`1> zawiera logikę w celu używania każdego odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-210">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory`1> has logic to use each appropriately.</span></span> <span data-ttu-id="9fcc8-211">`null` Nazwane opcja jest używana do Docieraj do wszystkich wystąpień nazwanych, zamiast określonego nazwanego wystąpienia (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> ta Konwencja).</span><span class="sxs-lookup"><span data-stu-id="9fcc8-211">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="9fcc8-212">OptionsBuilder interfejsu API</span><span class="sxs-lookup"><span data-stu-id="9fcc8-212">OptionsBuilder API</span></span>

<span data-ttu-id="9fcc8-213"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> Służy do konfigurowania `TOptions` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-213"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="9fcc8-214">`OptionsBuilder` usprawnia tworzenie o nazwie opcji, ponieważ jest tylko jeden parametr do początkowego `AddOptions<TOptions>(string optionsName)` wywoływać zamiast znajdujących się we wszystkich kolejnych wywołań.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-214">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="9fcc8-215">Opcje sprawdzania poprawności i `ConfigureOptions` przeciążenia, które akceptują zależności usług, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-215">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="configurelttoptions-tdep1--tdep4gt-method"></a><span data-ttu-id="9fcc8-216">Konfigurowanie&lt;TOptions, TDep1,... TDep4&gt; — metoda</span><span class="sxs-lookup"><span data-stu-id="9fcc8-216">Configure&lt;TOptions, TDep1, ... TDep4&gt; method</span></span>

<span data-ttu-id="9fcc8-217">Za pomocą usług z DI, aby skonfigurować opcje implementując `IConfigure[Named]Options` w standardowy sposób jest pełne.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-217">Using services from DI to configure options by implementing `IConfigure[Named]Options` in a boilerplate manner is verbose.</span></span> <span data-ttu-id="9fcc8-218">Przeciążenia `ConfigureOptions` na `OptionsBuilder<TOptions>` pozwalają na korzystanie z usług do pięciu do konfigurowania opcji:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-218">Overloads for `ConfigureOptions` on `OptionsBuilder<TOptions>` allow you to use up to five services to configure options:</span></span>

```csharp
services.AddOptions<MyOptions>("optionalName")
    .Configure<Service1, Service2, Service3, Service4, Service5>(
        (o, s, s2, s3, s4, s5) => 
            o.Property = DoSomethingWith(s, s2, s3, s4, s5));
```

<span data-ttu-id="9fcc8-219">Przeciążenie rejestruje rodzajowy przejściowe <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, która ma Konstruktor, który akceptuje typy Usługa ogólna określony.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-219">The overload registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, which has a constructor that accepts the generic service types specified.</span></span> 

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="9fcc8-220">Opcje weryfikacji</span><span class="sxs-lookup"><span data-stu-id="9fcc8-220">Options validation</span></span>

<span data-ttu-id="9fcc8-221">Opcje weryfikacji umożliwia sprawdzenie poprawności opcji, po skonfigurowaniu opcji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-221">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="9fcc8-222">Wywołaj `Validate` przy użyciu metody sprawdzania poprawności, która zwraca `true` Jeśli opcje są prawidłowe i `false` Jeśli nie są prawidłowe:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-222">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="9fcc8-223">Poprzedni przykład ustawia wystąpienia nazwanego opcji `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-223">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="9fcc8-224">Wystąpienie domyślne opcje to `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-224">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="9fcc8-225">Sprawdzanie poprawności jest uruchamiany, gdy tworzone jest wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-225">Validation runs when the options instance is created.</span></span> <span data-ttu-id="9fcc8-226">Wystąpienie opcji jest gwarantowane do przekazania razem pierwszego sprawdzania poprawności, gdy jest on dostępny.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-226">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9fcc8-227">Opcje sprawdzania poprawności nie je przed nieprzewidzianymi uniemożliwiającą modyfikacje opcje po opcji są wstępnie skonfigurowane i zweryfikowane.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-227">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="9fcc8-228">`Validate` Metoda przyjmuje `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-228">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="9fcc8-229">Aby w pełni dostosować sprawdzanie poprawności, należy zaimplementować `IValidateOptions<TOptions>`, co umożliwia:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-229">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="9fcc8-230">Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="9fcc8-230">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="9fcc8-231">Sprawdzanie poprawności, który jest zależny od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="9fcc8-231">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="9fcc8-232">`IValidateOptions` sprawdza poprawność:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-232">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="9fcc8-233">Określonego nazwanego wystąpienia opcje.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-233">A specific named options instance.</span></span>
* <span data-ttu-id="9fcc8-234">Wszystkie opcje, kiedy `name` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-234">All options when `name` is `null`.</span></span>

<span data-ttu-id="9fcc8-235">Zwróć `ValidateOptionsResult` z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-235">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="9fcc8-236">Data sprawdzania poprawności opartego na adnotacji jest dostępne z [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) pakietu przez wywołanie metody `ValidateDataAnnotations` metody `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-236">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the `ValidateDataAnnotations` method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="9fcc8-237">`Microsoft.Extensions.Options.DataAnnotations` znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2,2 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="9fcc8-237">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 or later).</span></span>

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

<span data-ttu-id="9fcc8-238">Weryfikacja eager (po awarii szybkie przy uruchamianiu) jest pod uwagę w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-238">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

::: moniker-end

## <a name="options-post-configuration"></a><span data-ttu-id="9fcc8-239">Opcje zadania po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="9fcc8-239">Options post-configuration</span></span>

<span data-ttu-id="9fcc8-240">Ustaw po konfiguracji za pomocą <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-240">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>.</span></span> <span data-ttu-id="9fcc8-241">Zadania po konfiguracji jest uruchamiana po wszystkich <xref:Microsoft.Extensions.Options.IConfigureOptions`1> występuje konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-241">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions`1> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="9fcc8-242"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępna po konfigurowania opcji o nazwie:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-242"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="9fcc8-243">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> po skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="9fcc8-243">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="9fcc8-244">Uzyskiwanie dostępu do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="9fcc8-244">Accessing options during startup</span></span>

<span data-ttu-id="9fcc8-245"><xref:Microsoft.Extensions.Options.IOptions`1> i <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed `Configure` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-245"><xref:Microsoft.Extensions.Options.IOptions`1> and <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="9fcc8-246">Nie używaj <xref:Microsoft.Extensions.Options.IOptions`1> lub <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-246">Don't use <xref:Microsoft.Extensions.Options.IOptions`1> or <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9fcc8-247">Stan opcje niezgodne mogą występować z powodu zamawiania rejestracji usługi.</span><span class="sxs-lookup"><span data-stu-id="9fcc8-247">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9fcc8-248">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9fcc8-248">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
