---
title: Options pattern in ASP.NET Core
author: guardrex
description: Discover how to use the options pattern to represent groups of related settings in ASP.NET Core apps.
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
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="1222d-103">Options pattern in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1222d-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="1222d-104">By [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1222d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1222d-105">The options pattern uses classes to represent groups of related settings.</span><span class="sxs-lookup"><span data-stu-id="1222d-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="1222d-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span><span class="sxs-lookup"><span data-stu-id="1222d-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="1222d-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span><span class="sxs-lookup"><span data-stu-id="1222d-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="1222d-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span><span class="sxs-lookup"><span data-stu-id="1222d-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="1222d-109">Options also provide a mechanism to validate configuration data.</span><span class="sxs-lookup"><span data-stu-id="1222d-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="1222d-110">For more information, see the [Options validation](#options-validation) section.</span><span class="sxs-lookup"><span data-stu-id="1222d-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="1222d-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1222d-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="1222d-112">Package</span><span class="sxs-lookup"><span data-stu-id="1222d-112">Package</span></span>

<span data-ttu-id="1222d-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span><span class="sxs-lookup"><span data-stu-id="1222d-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="1222d-114">Options interfaces</span><span class="sxs-lookup"><span data-stu-id="1222d-114">Options interfaces</span></span>

<span data-ttu-id="1222d-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="1222d-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span><span class="sxs-lookup"><span data-stu-id="1222d-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="1222d-117">Change notifications</span><span class="sxs-lookup"><span data-stu-id="1222d-117">Change notifications</span></span>
* [<span data-ttu-id="1222d-118">Named options</span><span class="sxs-lookup"><span data-stu-id="1222d-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="1222d-119">Reloadable configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="1222d-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="1222d-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="1222d-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span><span class="sxs-lookup"><span data-stu-id="1222d-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="1222d-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="1222d-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span><span class="sxs-lookup"><span data-stu-id="1222d-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="1222d-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span><span class="sxs-lookup"><span data-stu-id="1222d-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="1222d-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span><span class="sxs-lookup"><span data-stu-id="1222d-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="1222d-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="1222d-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="1222d-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="1222d-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="1222d-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="1222d-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span><span class="sxs-lookup"><span data-stu-id="1222d-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="1222d-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span><span class="sxs-lookup"><span data-stu-id="1222d-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="1222d-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span><span class="sxs-lookup"><span data-stu-id="1222d-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="1222d-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span><span class="sxs-lookup"><span data-stu-id="1222d-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="1222d-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="1222d-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="1222d-135">General options configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-135">General options configuration</span></span>

<span data-ttu-id="1222d-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="1222d-137">An options class must be non-abstract with a public parameterless constructor.</span><span class="sxs-lookup"><span data-stu-id="1222d-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="1222d-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span><span class="sxs-lookup"><span data-stu-id="1222d-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="1222d-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span><span class="sxs-lookup"><span data-stu-id="1222d-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="1222d-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="1222d-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span><span class="sxs-lookup"><span data-stu-id="1222d-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="1222d-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="1222d-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span><span class="sxs-lookup"><span data-stu-id="1222d-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="1222d-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="1222d-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="1222d-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span><span class="sxs-lookup"><span data-stu-id="1222d-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="1222d-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="1222d-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="1222d-147">Configure simple options with a delegate</span><span class="sxs-lookup"><span data-stu-id="1222d-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="1222d-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="1222d-149">Use a delegate to set options values.</span><span class="sxs-lookup"><span data-stu-id="1222d-149">Use a delegate to set options values.</span></span> <span data-ttu-id="1222d-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="1222d-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="1222d-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="1222d-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="1222d-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="1222d-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1222d-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="1222d-154">You can add multiple configuration providers.</span><span class="sxs-lookup"><span data-stu-id="1222d-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="1222d-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span><span class="sxs-lookup"><span data-stu-id="1222d-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="1222d-156">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1222d-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="1222d-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span><span class="sxs-lookup"><span data-stu-id="1222d-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="1222d-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span><span class="sxs-lookup"><span data-stu-id="1222d-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="1222d-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span><span class="sxs-lookup"><span data-stu-id="1222d-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="1222d-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="1222d-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="1222d-161">Suboptions configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-161">Suboptions configuration</span></span>

<span data-ttu-id="1222d-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="1222d-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span><span class="sxs-lookup"><span data-stu-id="1222d-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="1222d-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span><span class="sxs-lookup"><span data-stu-id="1222d-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="1222d-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="1222d-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="1222d-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1222d-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="1222d-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="1222d-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="1222d-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="1222d-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="1222d-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span><span class="sxs-lookup"><span data-stu-id="1222d-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="1222d-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="1222d-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="1222d-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="1222d-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="1222d-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span><span class="sxs-lookup"><span data-stu-id="1222d-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="1222d-174">Options injection</span><span class="sxs-lookup"><span data-stu-id="1222d-174">Options injection</span></span>

<span data-ttu-id="1222d-175">Options injection is demonstrated as Example &num;4 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-175">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="1222d-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span><span class="sxs-lookup"><span data-stu-id="1222d-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="1222d-177">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span><span class="sxs-lookup"><span data-stu-id="1222d-177">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="1222d-178">A page or view model.</span><span class="sxs-lookup"><span data-stu-id="1222d-178">A page or view model.</span></span>

<span data-ttu-id="1222d-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="1222d-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span><span class="sxs-lookup"><span data-stu-id="1222d-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="1222d-181">When the app is run, the options values are shown in the rendered page:</span><span class="sxs-lookup"><span data-stu-id="1222d-181">When the app is run, the options values are shown in the rendered page:</span></span>

![Options values Option1: value1_from_json and Option2: -1 are loaded from the model and by injection into the view.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="1222d-183">Reload configuration data with IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="1222d-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="1222d-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="1222d-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span><span class="sxs-lookup"><span data-stu-id="1222d-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="1222d-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span><span class="sxs-lookup"><span data-stu-id="1222d-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="1222d-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span><span class="sxs-lookup"><span data-stu-id="1222d-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="1222d-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span><span class="sxs-lookup"><span data-stu-id="1222d-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="1222d-189">Options snapshots are designed for use with transient and scoped dependencies.</span><span class="sxs-lookup"><span data-stu-id="1222d-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="1222d-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="1222d-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="1222d-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span><span class="sxs-lookup"><span data-stu-id="1222d-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="1222d-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="1222d-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="1222d-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span><span class="sxs-lookup"><span data-stu-id="1222d-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="1222d-194">Save the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="1222d-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="1222d-195">Refresh the browser to see that the options values are updated:</span><span class="sxs-lookup"><span data-stu-id="1222d-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="1222d-196">Named options support with IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="1222d-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="1222d-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="1222d-198">*Named options* support allows the app to distinguish between named options configurations.</span><span class="sxs-lookup"><span data-stu-id="1222d-198">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="1222d-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span><span class="sxs-lookup"><span data-stu-id="1222d-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="1222d-200">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-200">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="1222d-201">Running the sample app, the named options are returned:</span><span class="sxs-lookup"><span data-stu-id="1222d-201">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="1222d-202">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="1222d-202">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="1222d-203">`named_options_2` values are provided by:</span><span class="sxs-lookup"><span data-stu-id="1222d-203">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="1222d-204">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span><span class="sxs-lookup"><span data-stu-id="1222d-204">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="1222d-205">The default value for `Option2` provided by the `MyOptions` class.</span><span class="sxs-lookup"><span data-stu-id="1222d-205">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="1222d-206">Configure all options with the ConfigureAll method</span><span class="sxs-lookup"><span data-stu-id="1222d-206">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="1222d-207">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span><span class="sxs-lookup"><span data-stu-id="1222d-207">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="1222d-208">The following code configures `Option1` for all configuration instances with a common value.</span><span class="sxs-lookup"><span data-stu-id="1222d-208">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="1222d-209">Add the following code manually to the `Startup.ConfigureServices` method:</span><span class="sxs-lookup"><span data-stu-id="1222d-209">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="1222d-210">Running the sample app after adding the code produces the following result:</span><span class="sxs-lookup"><span data-stu-id="1222d-210">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="1222d-211">All options are named instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-211">All options are named instances.</span></span> <span data-ttu-id="1222d-212">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="1222d-212">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="1222d-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="1222d-214">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span><span class="sxs-lookup"><span data-stu-id="1222d-214">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="1222d-215">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span><span class="sxs-lookup"><span data-stu-id="1222d-215">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="1222d-216">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="1222d-216">OptionsBuilder API</span></span>

<span data-ttu-id="1222d-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="1222d-218">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span><span class="sxs-lookup"><span data-stu-id="1222d-218">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="1222d-219">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1222d-219">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="1222d-220">Use DI services to configure options</span><span class="sxs-lookup"><span data-stu-id="1222d-220">Use DI services to configure options</span></span>

<span data-ttu-id="1222d-221">You can access other services from dependency injection while configuring options in two ways:</span><span class="sxs-lookup"><span data-stu-id="1222d-221">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="1222d-222">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="1222d-222">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="1222d-223">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span><span class="sxs-lookup"><span data-stu-id="1222d-223">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="1222d-224">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span><span class="sxs-lookup"><span data-stu-id="1222d-224">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="1222d-225">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span><span class="sxs-lookup"><span data-stu-id="1222d-225">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="1222d-226">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="1222d-226">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="1222d-227">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span><span class="sxs-lookup"><span data-stu-id="1222d-227">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="1222d-228">Options validation</span><span class="sxs-lookup"><span data-stu-id="1222d-228">Options validation</span></span>

<span data-ttu-id="1222d-229">Options validation allows you to validate options when options are configured.</span><span class="sxs-lookup"><span data-stu-id="1222d-229">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="1222d-230">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span><span class="sxs-lookup"><span data-stu-id="1222d-230">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="1222d-231">The preceding example sets the named options instance to `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="1222d-231">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="1222d-232">The default options instance is `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="1222d-232">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="1222d-233">Validation runs when the options instance is created.</span><span class="sxs-lookup"><span data-stu-id="1222d-233">Validation runs when the options instance is created.</span></span> <span data-ttu-id="1222d-234">Your options instance is guaranteed to pass validation the first time it's accessed.</span><span class="sxs-lookup"><span data-stu-id="1222d-234">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1222d-235">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span><span class="sxs-lookup"><span data-stu-id="1222d-235">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="1222d-236">The `Validate` method accepts a `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="1222d-236">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="1222d-237">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span><span class="sxs-lookup"><span data-stu-id="1222d-237">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="1222d-238">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="1222d-238">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="1222d-239">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="1222d-239">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="1222d-240">`IValidateOptions` validates:</span><span class="sxs-lookup"><span data-stu-id="1222d-240">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="1222d-241">A specific named options instance.</span><span class="sxs-lookup"><span data-stu-id="1222d-241">A specific named options instance.</span></span>
* <span data-ttu-id="1222d-242">All options when `name` is `null`.</span><span class="sxs-lookup"><span data-stu-id="1222d-242">All options when `name` is `null`.</span></span>

<span data-ttu-id="1222d-243">Return a `ValidateOptionsResult` from your implementation of the interface:</span><span class="sxs-lookup"><span data-stu-id="1222d-243">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="1222d-244">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="1222d-244">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="1222d-245">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span><span class="sxs-lookup"><span data-stu-id="1222d-245">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

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

<span data-ttu-id="1222d-246">Eager validation (fail fast at startup) is under consideration for a future release.</span><span class="sxs-lookup"><span data-stu-id="1222d-246">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="1222d-247">Options post-configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-247">Options post-configuration</span></span>

<span data-ttu-id="1222d-248">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-248">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="1222d-249">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span><span class="sxs-lookup"><span data-stu-id="1222d-249">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="1222d-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span><span class="sxs-lookup"><span data-stu-id="1222d-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="1222d-251">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span><span class="sxs-lookup"><span data-stu-id="1222d-251">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="1222d-252">Accessing options during startup</span><span class="sxs-lookup"><span data-stu-id="1222d-252">Accessing options during startup</span></span>

<span data-ttu-id="1222d-253"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span><span class="sxs-lookup"><span data-stu-id="1222d-253"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="1222d-254">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1222d-254">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1222d-255">An inconsistent options state may exist due to the ordering of service registrations.</span><span class="sxs-lookup"><span data-stu-id="1222d-255">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="1222d-256">The options pattern uses classes to represent groups of related settings.</span><span class="sxs-lookup"><span data-stu-id="1222d-256">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="1222d-257">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span><span class="sxs-lookup"><span data-stu-id="1222d-257">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="1222d-258">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span><span class="sxs-lookup"><span data-stu-id="1222d-258">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="1222d-259">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span><span class="sxs-lookup"><span data-stu-id="1222d-259">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="1222d-260">Options also provide a mechanism to validate configuration data.</span><span class="sxs-lookup"><span data-stu-id="1222d-260">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="1222d-261">For more information, see the [Options validation](#options-validation) section.</span><span class="sxs-lookup"><span data-stu-id="1222d-261">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="1222d-262">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1222d-262">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1222d-263">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1222d-263">Prerequisites</span></span>

<span data-ttu-id="1222d-264">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span><span class="sxs-lookup"><span data-stu-id="1222d-264">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="1222d-265">Options interfaces</span><span class="sxs-lookup"><span data-stu-id="1222d-265">Options interfaces</span></span>

<span data-ttu-id="1222d-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="1222d-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span><span class="sxs-lookup"><span data-stu-id="1222d-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="1222d-268">Change notifications</span><span class="sxs-lookup"><span data-stu-id="1222d-268">Change notifications</span></span>
* [<span data-ttu-id="1222d-269">Named options</span><span class="sxs-lookup"><span data-stu-id="1222d-269">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="1222d-270">Reloadable configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-270">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="1222d-271">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="1222d-271">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="1222d-272">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span><span class="sxs-lookup"><span data-stu-id="1222d-272">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="1222d-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="1222d-274">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span><span class="sxs-lookup"><span data-stu-id="1222d-274">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="1222d-275">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span><span class="sxs-lookup"><span data-stu-id="1222d-275">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="1222d-276">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span><span class="sxs-lookup"><span data-stu-id="1222d-276">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="1222d-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="1222d-278">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="1222d-278">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="1222d-279">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="1222d-279">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="1222d-280">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span><span class="sxs-lookup"><span data-stu-id="1222d-280">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="1222d-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span><span class="sxs-lookup"><span data-stu-id="1222d-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="1222d-282">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span><span class="sxs-lookup"><span data-stu-id="1222d-282">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="1222d-283"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span><span class="sxs-lookup"><span data-stu-id="1222d-283"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="1222d-284">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-284">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="1222d-285">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-285">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="1222d-286">General options configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-286">General options configuration</span></span>

<span data-ttu-id="1222d-287">General options configuration is demonstrated as Example &num;1 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-287">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="1222d-288">An options class must be non-abstract with a public parameterless constructor.</span><span class="sxs-lookup"><span data-stu-id="1222d-288">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="1222d-289">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span><span class="sxs-lookup"><span data-stu-id="1222d-289">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="1222d-290">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span><span class="sxs-lookup"><span data-stu-id="1222d-290">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="1222d-291">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-291">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="1222d-292">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span><span class="sxs-lookup"><span data-stu-id="1222d-292">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="1222d-293">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-293">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="1222d-294">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span><span class="sxs-lookup"><span data-stu-id="1222d-294">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="1222d-295">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="1222d-295">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="1222d-296">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span><span class="sxs-lookup"><span data-stu-id="1222d-296">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="1222d-297">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="1222d-297">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="1222d-298">Configure simple options with a delegate</span><span class="sxs-lookup"><span data-stu-id="1222d-298">Configure simple options with a delegate</span></span>

<span data-ttu-id="1222d-299">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-299">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="1222d-300">Use a delegate to set options values.</span><span class="sxs-lookup"><span data-stu-id="1222d-300">Use a delegate to set options values.</span></span> <span data-ttu-id="1222d-301">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-301">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="1222d-302">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="1222d-302">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="1222d-303">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="1222d-303">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="1222d-304">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1222d-304">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="1222d-305">You can add multiple configuration providers.</span><span class="sxs-lookup"><span data-stu-id="1222d-305">You can add multiple configuration providers.</span></span> <span data-ttu-id="1222d-306">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span><span class="sxs-lookup"><span data-stu-id="1222d-306">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="1222d-307">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1222d-307">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="1222d-308">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span><span class="sxs-lookup"><span data-stu-id="1222d-308">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="1222d-309">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span><span class="sxs-lookup"><span data-stu-id="1222d-309">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="1222d-310">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span><span class="sxs-lookup"><span data-stu-id="1222d-310">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="1222d-311">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="1222d-311">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="1222d-312">Suboptions configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-312">Suboptions configuration</span></span>

<span data-ttu-id="1222d-313">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-313">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="1222d-314">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span><span class="sxs-lookup"><span data-stu-id="1222d-314">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="1222d-315">Parts of the app that require configuration values should only have access to the configuration values that they use.</span><span class="sxs-lookup"><span data-stu-id="1222d-315">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="1222d-316">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="1222d-316">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="1222d-317">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1222d-317">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="1222d-318">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="1222d-318">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="1222d-319">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="1222d-319">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="1222d-320">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span><span class="sxs-lookup"><span data-stu-id="1222d-320">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="1222d-321">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="1222d-321">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="1222d-322">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-322">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="1222d-323">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-323">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="1222d-324">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span><span class="sxs-lookup"><span data-stu-id="1222d-324">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="1222d-325">Options injection</span><span class="sxs-lookup"><span data-stu-id="1222d-325">Options injection</span></span>

<span data-ttu-id="1222d-326">Options injection is demonstrated as Example &num;4 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-326">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="1222d-327">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span><span class="sxs-lookup"><span data-stu-id="1222d-327">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="1222d-328">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span><span class="sxs-lookup"><span data-stu-id="1222d-328">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="1222d-329">A page or view model.</span><span class="sxs-lookup"><span data-stu-id="1222d-329">A page or view model.</span></span>

<span data-ttu-id="1222d-330">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-330">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="1222d-331">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span><span class="sxs-lookup"><span data-stu-id="1222d-331">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="1222d-332">When the app is run, the options values are shown in the rendered page:</span><span class="sxs-lookup"><span data-stu-id="1222d-332">When the app is run, the options values are shown in the rendered page:</span></span>

![Options values Option1: value1_from_json and Option2: -1 are loaded from the model and by injection into the view.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="1222d-334">Reload configuration data with IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="1222d-334">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="1222d-335">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-335">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="1222d-336">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span><span class="sxs-lookup"><span data-stu-id="1222d-336">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="1222d-337">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span><span class="sxs-lookup"><span data-stu-id="1222d-337">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="1222d-338">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span><span class="sxs-lookup"><span data-stu-id="1222d-338">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="1222d-339">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span><span class="sxs-lookup"><span data-stu-id="1222d-339">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="1222d-340">Options snapshots are designed for use with transient and scoped dependencies.</span><span class="sxs-lookup"><span data-stu-id="1222d-340">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="1222d-341">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="1222d-341">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="1222d-342">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span><span class="sxs-lookup"><span data-stu-id="1222d-342">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="1222d-343">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="1222d-343">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="1222d-344">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span><span class="sxs-lookup"><span data-stu-id="1222d-344">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="1222d-345">Save the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="1222d-345">Save the *appsettings.json* file.</span></span> <span data-ttu-id="1222d-346">Refresh the browser to see that the options values are updated:</span><span class="sxs-lookup"><span data-stu-id="1222d-346">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="1222d-347">Named options support with IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="1222d-347">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="1222d-348">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-348">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="1222d-349">*Named options* support allows the app to distinguish between named options configurations.</span><span class="sxs-lookup"><span data-stu-id="1222d-349">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="1222d-350">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span><span class="sxs-lookup"><span data-stu-id="1222d-350">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="1222d-351">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-351">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="1222d-352">Running the sample app, the named options are returned:</span><span class="sxs-lookup"><span data-stu-id="1222d-352">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="1222d-353">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="1222d-353">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="1222d-354">`named_options_2` values are provided by:</span><span class="sxs-lookup"><span data-stu-id="1222d-354">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="1222d-355">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span><span class="sxs-lookup"><span data-stu-id="1222d-355">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="1222d-356">The default value for `Option2` provided by the `MyOptions` class.</span><span class="sxs-lookup"><span data-stu-id="1222d-356">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="1222d-357">Configure all options with the ConfigureAll method</span><span class="sxs-lookup"><span data-stu-id="1222d-357">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="1222d-358">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span><span class="sxs-lookup"><span data-stu-id="1222d-358">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="1222d-359">The following code configures `Option1` for all configuration instances with a common value.</span><span class="sxs-lookup"><span data-stu-id="1222d-359">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="1222d-360">Add the following code manually to the `Startup.ConfigureServices` method:</span><span class="sxs-lookup"><span data-stu-id="1222d-360">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="1222d-361">Running the sample app after adding the code produces the following result:</span><span class="sxs-lookup"><span data-stu-id="1222d-361">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="1222d-362">All options are named instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-362">All options are named instances.</span></span> <span data-ttu-id="1222d-363">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="1222d-363">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="1222d-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="1222d-365">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span><span class="sxs-lookup"><span data-stu-id="1222d-365">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="1222d-366">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span><span class="sxs-lookup"><span data-stu-id="1222d-366">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="1222d-367">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="1222d-367">OptionsBuilder API</span></span>

<span data-ttu-id="1222d-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="1222d-369">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span><span class="sxs-lookup"><span data-stu-id="1222d-369">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="1222d-370">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1222d-370">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="1222d-371">Use DI services to configure options</span><span class="sxs-lookup"><span data-stu-id="1222d-371">Use DI services to configure options</span></span>

<span data-ttu-id="1222d-372">You can access other services from dependency injection while configuring options in two ways:</span><span class="sxs-lookup"><span data-stu-id="1222d-372">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="1222d-373">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="1222d-373">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="1222d-374">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span><span class="sxs-lookup"><span data-stu-id="1222d-374">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="1222d-375">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span><span class="sxs-lookup"><span data-stu-id="1222d-375">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="1222d-376">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span><span class="sxs-lookup"><span data-stu-id="1222d-376">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="1222d-377">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="1222d-377">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="1222d-378">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span><span class="sxs-lookup"><span data-stu-id="1222d-378">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="1222d-379">Options validation</span><span class="sxs-lookup"><span data-stu-id="1222d-379">Options validation</span></span>

<span data-ttu-id="1222d-380">Options validation allows you to validate options when options are configured.</span><span class="sxs-lookup"><span data-stu-id="1222d-380">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="1222d-381">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span><span class="sxs-lookup"><span data-stu-id="1222d-381">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="1222d-382">The preceding example sets the named options instance to `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="1222d-382">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="1222d-383">The default options instance is `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="1222d-383">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="1222d-384">Validation runs when the options instance is created.</span><span class="sxs-lookup"><span data-stu-id="1222d-384">Validation runs when the options instance is created.</span></span> <span data-ttu-id="1222d-385">Your options instance is guaranteed to pass validation the first time it's accessed.</span><span class="sxs-lookup"><span data-stu-id="1222d-385">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1222d-386">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span><span class="sxs-lookup"><span data-stu-id="1222d-386">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="1222d-387">The `Validate` method accepts a `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="1222d-387">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="1222d-388">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span><span class="sxs-lookup"><span data-stu-id="1222d-388">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="1222d-389">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="1222d-389">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="1222d-390">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="1222d-390">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="1222d-391">`IValidateOptions` validates:</span><span class="sxs-lookup"><span data-stu-id="1222d-391">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="1222d-392">A specific named options instance.</span><span class="sxs-lookup"><span data-stu-id="1222d-392">A specific named options instance.</span></span>
* <span data-ttu-id="1222d-393">All options when `name` is `null`.</span><span class="sxs-lookup"><span data-stu-id="1222d-393">All options when `name` is `null`.</span></span>

<span data-ttu-id="1222d-394">Return a `ValidateOptionsResult` from your implementation of the interface:</span><span class="sxs-lookup"><span data-stu-id="1222d-394">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="1222d-395">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="1222d-395">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="1222d-396">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1222d-396">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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

<span data-ttu-id="1222d-397">Eager validation (fail fast at startup) is under consideration for a future release.</span><span class="sxs-lookup"><span data-stu-id="1222d-397">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="1222d-398">Options post-configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-398">Options post-configuration</span></span>

<span data-ttu-id="1222d-399">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-399">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="1222d-400">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span><span class="sxs-lookup"><span data-stu-id="1222d-400">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="1222d-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span><span class="sxs-lookup"><span data-stu-id="1222d-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="1222d-402">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span><span class="sxs-lookup"><span data-stu-id="1222d-402">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="1222d-403">Accessing options during startup</span><span class="sxs-lookup"><span data-stu-id="1222d-403">Accessing options during startup</span></span>

<span data-ttu-id="1222d-404"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span><span class="sxs-lookup"><span data-stu-id="1222d-404"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="1222d-405">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1222d-405">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1222d-406">An inconsistent options state may exist due to the ordering of service registrations.</span><span class="sxs-lookup"><span data-stu-id="1222d-406">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="1222d-407">The options pattern uses classes to represent groups of related settings.</span><span class="sxs-lookup"><span data-stu-id="1222d-407">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="1222d-408">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span><span class="sxs-lookup"><span data-stu-id="1222d-408">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="1222d-409">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span><span class="sxs-lookup"><span data-stu-id="1222d-409">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="1222d-410">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span><span class="sxs-lookup"><span data-stu-id="1222d-410">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="1222d-411">Options also provide a mechanism to validate configuration data.</span><span class="sxs-lookup"><span data-stu-id="1222d-411">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="1222d-412">For more information, see the [Options validation](#options-validation) section.</span><span class="sxs-lookup"><span data-stu-id="1222d-412">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="1222d-413">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1222d-413">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1222d-414">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1222d-414">Prerequisites</span></span>

<span data-ttu-id="1222d-415">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span><span class="sxs-lookup"><span data-stu-id="1222d-415">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="1222d-416">Options interfaces</span><span class="sxs-lookup"><span data-stu-id="1222d-416">Options interfaces</span></span>

<span data-ttu-id="1222d-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="1222d-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span><span class="sxs-lookup"><span data-stu-id="1222d-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="1222d-419">Change notifications</span><span class="sxs-lookup"><span data-stu-id="1222d-419">Change notifications</span></span>
* [<span data-ttu-id="1222d-420">Named options</span><span class="sxs-lookup"><span data-stu-id="1222d-420">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="1222d-421">Reloadable configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-421">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="1222d-422">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="1222d-422">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="1222d-423">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span><span class="sxs-lookup"><span data-stu-id="1222d-423">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="1222d-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="1222d-425">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span><span class="sxs-lookup"><span data-stu-id="1222d-425">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="1222d-426">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span><span class="sxs-lookup"><span data-stu-id="1222d-426">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="1222d-427">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span><span class="sxs-lookup"><span data-stu-id="1222d-427">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="1222d-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="1222d-429">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="1222d-429">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="1222d-430">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="1222d-430">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="1222d-431">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span><span class="sxs-lookup"><span data-stu-id="1222d-431">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="1222d-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span><span class="sxs-lookup"><span data-stu-id="1222d-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="1222d-433">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span><span class="sxs-lookup"><span data-stu-id="1222d-433">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="1222d-434"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span><span class="sxs-lookup"><span data-stu-id="1222d-434"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="1222d-435">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-435">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="1222d-436">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-436">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="1222d-437">General options configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-437">General options configuration</span></span>

<span data-ttu-id="1222d-438">General options configuration is demonstrated as Example &num;1 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-438">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="1222d-439">An options class must be non-abstract with a public parameterless constructor.</span><span class="sxs-lookup"><span data-stu-id="1222d-439">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="1222d-440">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span><span class="sxs-lookup"><span data-stu-id="1222d-440">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="1222d-441">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span><span class="sxs-lookup"><span data-stu-id="1222d-441">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="1222d-442">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-442">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="1222d-443">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span><span class="sxs-lookup"><span data-stu-id="1222d-443">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="1222d-444">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-444">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="1222d-445">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span><span class="sxs-lookup"><span data-stu-id="1222d-445">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="1222d-446">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="1222d-446">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="1222d-447">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span><span class="sxs-lookup"><span data-stu-id="1222d-447">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="1222d-448">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="1222d-448">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="1222d-449">Configure simple options with a delegate</span><span class="sxs-lookup"><span data-stu-id="1222d-449">Configure simple options with a delegate</span></span>

<span data-ttu-id="1222d-450">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-450">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="1222d-451">Use a delegate to set options values.</span><span class="sxs-lookup"><span data-stu-id="1222d-451">Use a delegate to set options values.</span></span> <span data-ttu-id="1222d-452">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-452">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="1222d-453">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="1222d-453">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="1222d-454">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="1222d-454">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="1222d-455">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1222d-455">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="1222d-456">You can add multiple configuration providers.</span><span class="sxs-lookup"><span data-stu-id="1222d-456">You can add multiple configuration providers.</span></span> <span data-ttu-id="1222d-457">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span><span class="sxs-lookup"><span data-stu-id="1222d-457">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="1222d-458">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1222d-458">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="1222d-459">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span><span class="sxs-lookup"><span data-stu-id="1222d-459">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="1222d-460">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span><span class="sxs-lookup"><span data-stu-id="1222d-460">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="1222d-461">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span><span class="sxs-lookup"><span data-stu-id="1222d-461">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="1222d-462">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span><span class="sxs-lookup"><span data-stu-id="1222d-462">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="1222d-463">Suboptions configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-463">Suboptions configuration</span></span>

<span data-ttu-id="1222d-464">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-464">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="1222d-465">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span><span class="sxs-lookup"><span data-stu-id="1222d-465">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="1222d-466">Parts of the app that require configuration values should only have access to the configuration values that they use.</span><span class="sxs-lookup"><span data-stu-id="1222d-466">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="1222d-467">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="1222d-467">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="1222d-468">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1222d-468">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="1222d-469">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span><span class="sxs-lookup"><span data-stu-id="1222d-469">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="1222d-470">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="1222d-470">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="1222d-471">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span><span class="sxs-lookup"><span data-stu-id="1222d-471">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="1222d-472">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="1222d-472">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="1222d-473">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-473">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="1222d-474">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-474">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="1222d-475">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span><span class="sxs-lookup"><span data-stu-id="1222d-475">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="1222d-476">Options provided by a view model or with direct view injection</span><span class="sxs-lookup"><span data-stu-id="1222d-476">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="1222d-477">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-477">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="1222d-478">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-478">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="1222d-479">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span><span class="sxs-lookup"><span data-stu-id="1222d-479">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="1222d-480">When the app is run, the options values are shown in the rendered page:</span><span class="sxs-lookup"><span data-stu-id="1222d-480">When the app is run, the options values are shown in the rendered page:</span></span>

![Options values Option1: value1_from_json and Option2: -1 are loaded from the model and by injection into the view.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="1222d-482">Reload configuration data with IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="1222d-482">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="1222d-483">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-483">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="1222d-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span><span class="sxs-lookup"><span data-stu-id="1222d-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="1222d-485">Options are computed once per request when accessed and cached for the lifetime of the request.</span><span class="sxs-lookup"><span data-stu-id="1222d-485">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="1222d-486">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="1222d-486">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="1222d-487">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span><span class="sxs-lookup"><span data-stu-id="1222d-487">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="1222d-488">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="1222d-488">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="1222d-489">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span><span class="sxs-lookup"><span data-stu-id="1222d-489">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="1222d-490">Save the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="1222d-490">Save the *appsettings.json* file.</span></span> <span data-ttu-id="1222d-491">Refresh the browser to see that the options values are updated:</span><span class="sxs-lookup"><span data-stu-id="1222d-491">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="1222d-492">Named options support with IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="1222d-492">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="1222d-493">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span><span class="sxs-lookup"><span data-stu-id="1222d-493">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="1222d-494">*Named options* support allows the app to distinguish between named options configurations.</span><span class="sxs-lookup"><span data-stu-id="1222d-494">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="1222d-495">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span><span class="sxs-lookup"><span data-stu-id="1222d-495">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="1222d-496">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1222d-496">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="1222d-497">Running the sample app, the named options are returned:</span><span class="sxs-lookup"><span data-stu-id="1222d-497">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="1222d-498">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span><span class="sxs-lookup"><span data-stu-id="1222d-498">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="1222d-499">`named_options_2` values are provided by:</span><span class="sxs-lookup"><span data-stu-id="1222d-499">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="1222d-500">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span><span class="sxs-lookup"><span data-stu-id="1222d-500">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="1222d-501">The default value for `Option2` provided by the `MyOptions` class.</span><span class="sxs-lookup"><span data-stu-id="1222d-501">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="1222d-502">Configure all options with the ConfigureAll method</span><span class="sxs-lookup"><span data-stu-id="1222d-502">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="1222d-503">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span><span class="sxs-lookup"><span data-stu-id="1222d-503">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="1222d-504">The following code configures `Option1` for all configuration instances with a common value.</span><span class="sxs-lookup"><span data-stu-id="1222d-504">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="1222d-505">Add the following code manually to the `Startup.ConfigureServices` method:</span><span class="sxs-lookup"><span data-stu-id="1222d-505">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="1222d-506">Running the sample app after adding the code produces the following result:</span><span class="sxs-lookup"><span data-stu-id="1222d-506">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="1222d-507">All options are named instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-507">All options are named instances.</span></span> <span data-ttu-id="1222d-508">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="1222d-508">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="1222d-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="1222d-510">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span><span class="sxs-lookup"><span data-stu-id="1222d-510">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="1222d-511">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span><span class="sxs-lookup"><span data-stu-id="1222d-511">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="1222d-512">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="1222d-512">OptionsBuilder API</span></span>

<span data-ttu-id="1222d-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="1222d-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="1222d-514">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span><span class="sxs-lookup"><span data-stu-id="1222d-514">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="1222d-515">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1222d-515">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="1222d-516">Use DI services to configure options</span><span class="sxs-lookup"><span data-stu-id="1222d-516">Use DI services to configure options</span></span>

<span data-ttu-id="1222d-517">You can access other services from dependency injection while configuring options in two ways:</span><span class="sxs-lookup"><span data-stu-id="1222d-517">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="1222d-518">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="1222d-518">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="1222d-519">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span><span class="sxs-lookup"><span data-stu-id="1222d-519">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="1222d-520">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span><span class="sxs-lookup"><span data-stu-id="1222d-520">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="1222d-521">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span><span class="sxs-lookup"><span data-stu-id="1222d-521">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="1222d-522">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="1222d-522">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="1222d-523">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span><span class="sxs-lookup"><span data-stu-id="1222d-523">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="1222d-524">Options post-configuration</span><span class="sxs-lookup"><span data-stu-id="1222d-524">Options post-configuration</span></span>

<span data-ttu-id="1222d-525">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="1222d-525">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="1222d-526">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span><span class="sxs-lookup"><span data-stu-id="1222d-526">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="1222d-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span><span class="sxs-lookup"><span data-stu-id="1222d-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="1222d-528">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span><span class="sxs-lookup"><span data-stu-id="1222d-528">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="1222d-529">Accessing options during startup</span><span class="sxs-lookup"><span data-stu-id="1222d-529">Accessing options during startup</span></span>

<span data-ttu-id="1222d-530"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span><span class="sxs-lookup"><span data-stu-id="1222d-530"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="1222d-531">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1222d-531">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1222d-532">An inconsistent options state may exist due to the ordering of service registrations.</span><span class="sxs-lookup"><span data-stu-id="1222d-532">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="1222d-533">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1222d-533">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
