---
uid: config-builder
title: Konstruktorzy konfiguracji dla platformy ASP.NET
author: rick-anderson
description: Dowiedz się, jak uzyskać dane konfiguracyjne ze źródeł innych niż wartości web.config ze źródeł zewnętrznych.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148840"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="83a5a-103">Konstruktorzy konfiguracji dla platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="83a5a-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="83a5a-104">Przez [Molloy Autor: Stephen](https://github.com/StephenMolloy) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="83a5a-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="83a5a-105">Konstruktorzy konfiguracji mechanizmu nowoczesnych i elastyczne aplikacje platformy ASP.NET można pobrać wartości konfiguracji ze źródeł zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="83a5a-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="83a5a-106">Konstruktorzy konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="83a5a-106">Configuration builders:</span></span>

* <span data-ttu-id="83a5a-107">Są dostępne w programie .NET Framework 4.7.1 i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="83a5a-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="83a5a-108">Zapewniają elastyczny mechanizm do odczytywania wartości konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="83a5a-109">Adres niektóre podstawowe wymagania aplikacji, zgodnie z ich przenieść w kontenerze i ukierunkowane środowiska w chmurze.</span><span class="sxs-lookup"><span data-stu-id="83a5a-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="83a5a-110">Może służyć w celu zwiększenia ochrony danych konfiguracji, rysowania ze źródeł wcześniej niedostępne (na przykład usługi Azure Key Vault i zmiennymi środowiskowymi) w systemu konfiguracji platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="83a5a-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="83a5a-111">Konstruktorzy konfiguracji klucz wartość</span><span class="sxs-lookup"><span data-stu-id="83a5a-111">Key/value configuration builders</span></span>

<span data-ttu-id="83a5a-112">Typowy scenariusz, który może zostać obsłużony przez producentów konfiguracji jest zapewnienie mechanizmu zastępczy podstawowego klucza i wartości dla sekcji konfiguracji, które wzorca klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="83a5a-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="83a5a-113">.NET Framework koncepcji ConfigurationBuilders nie jest ograniczona do konkretnej konfiguracji sekcje lub wzorce.</span><span class="sxs-lookup"><span data-stu-id="83a5a-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="83a5a-114">Jednak wiele z konstruktorów konfiguracji w `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) działają w ramach wzorzec klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="83a5a-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="83a5a-115">Ustawienia Konstruktorzy konfiguracji klucz wartość</span><span class="sxs-lookup"><span data-stu-id="83a5a-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="83a5a-116">Następujące ustawienia mają zastosowanie do wszystkich Konstruktorzy konfiguracji klucz wartość w `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="83a5a-117">Tryb</span><span class="sxs-lookup"><span data-stu-id="83a5a-117">Mode</span></span>

<span data-ttu-id="83a5a-118">Konstruktorzy konfiguracji Użyj zewnętrznego źródła danych klucz wartość do wypełnienia wybrany klucz/wartość elementów konfiguracji systemu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="83a5a-119">W szczególności `<appSettings/>` i `<connectionStrings/>` sekcje otrzymywać specjalnego traktowania Konstruktorzy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="83a5a-120">Konstruktorzy pracy trzech trybów:</span><span class="sxs-lookup"><span data-stu-id="83a5a-120">The builders work in three modes:</span></span>

* <span data-ttu-id="83a5a-121">`Strict` — Domyślny tryb.</span><span class="sxs-lookup"><span data-stu-id="83a5a-121">`Strict` - The default mode.</span></span> <span data-ttu-id="83a5a-122">W tym trybie konstruktora konfiguracji działa tylko w sekcji znanych klucz/wartość przetwarzających konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="83a5a-123">`Strict` Tryb wylicza każdy klucz w sekcji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="83a5a-124">Jeśli dopasowany klucz zostanie znaleziony w zewnętrznego źródła:</span><span class="sxs-lookup"><span data-stu-id="83a5a-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="83a5a-125">Konstruktorzy konfiguracji Zastąp wartość w sekcji Konfiguracja wynikowej wartości z zewnętrznego źródła.</span><span class="sxs-lookup"><span data-stu-id="83a5a-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="83a5a-126">`Greedy` — W tym trybie jest ściśle powiązane z `Strict` trybu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="83a5a-127">Zamiast są ograniczone do kluczy, które już istnieją w oryginalnej konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="83a5a-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="83a5a-128">Konstruktorzy konfiguracji umożliwia dodanie wszystkich par klucz wartość z zewnętrznego źródła wynikową sekcję konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="83a5a-129">`Expand` — Działa w nieprzetworzonym kodzie XML przed jest analizowany na obiekt sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="83a5a-130">Jego można traktować jako rozszerzenie tokenów w ciągu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="83a5a-131">Dowolną część nieprzetworzonego ciągu XML, który pasuje do wzorca `${token}` jest kandydatem do rozszerzenia tokenu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="83a5a-132">Jeśli nie odpowiadająca wartość znajduje się w zewnętrznego źródła, token nie jest zmieniany.</span><span class="sxs-lookup"><span data-stu-id="83a5a-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="83a5a-133">Konstruktorzy w tym trybie nie są ograniczone do `<appSettings/>` i `<connectionStrings/>` sekcje.</span><span class="sxs-lookup"><span data-stu-id="83a5a-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="83a5a-134">Następujące znaczniki z *web.config* umożliwia [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) w `Strict` trybu:</span><span class="sxs-lookup"><span data-stu-id="83a5a-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="83a5a-135">Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` pokazano w poprzednim *web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="83a5a-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="83a5a-136">Powyższy kod będzie równa wartości właściwości:</span><span class="sxs-lookup"><span data-stu-id="83a5a-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="83a5a-137">Wartości w *web.config* plik, jeśli klucze nie są ustawione w zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="83a5a-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="83a5a-138">Wartości środowiska zmiennej, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="83a5a-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="83a5a-139">Na przykład `ServiceID` będzie zawierać:</span><span class="sxs-lookup"><span data-stu-id="83a5a-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="83a5a-140">"Identyfikator ServiceID wartość z pliku web.config", jeśli zmienna środowiskowa `ServiceID` nie jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="83a5a-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="83a5a-141">Wartość `ServiceID` środowiska zmiennej, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="83a5a-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="83a5a-142">Na poniższej ilustracji przedstawiono `<appSettings/>` kluczy/wartości z poprzednim *web.config* pliku zestawu w edytorze środowiska:</span><span class="sxs-lookup"><span data-stu-id="83a5a-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Edytor środowiska](config-builder/static/env.png)

<span data-ttu-id="83a5a-144">Uwaga: Może być konieczne zamknięciu i ponownym uruchomieniu programu Visual Studio, aby zobaczyć zmiany w zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="83a5a-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="83a5a-145">Obsługa prefiksu</span><span class="sxs-lookup"><span data-stu-id="83a5a-145">Prefix handling</span></span>

<span data-ttu-id="83a5a-146">Prefiksy klucza można uprościć klucze ustawienia, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="83a5a-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="83a5a-147">Konfiguracja środowiska .NET Framework jest złożona i zagnieżdżone.</span><span class="sxs-lookup"><span data-stu-id="83a5a-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="83a5a-148">Źródła zewnętrznego klucz/wartość często mają charakter podstawowy i płaską ze względu na charakter.</span><span class="sxs-lookup"><span data-stu-id="83a5a-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="83a5a-149">Na przykład zmienne środowiskowe nie są zagnieżdżone.</span><span class="sxs-lookup"><span data-stu-id="83a5a-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="83a5a-150">Użyć dowolnej z następujących metod iniekcję zarówno `<appSettings/>` i `<connectionStrings/>` do konfiguracji za pomocą zmiennych środowiskowych:</span><span class="sxs-lookup"><span data-stu-id="83a5a-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="83a5a-151">Za pomocą `EnvironmentConfigBuilder` w domyślnym `Strict` tryb i odpowiednie nazwy kluczy w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="83a5a-152">Poprzedni kod i znaczników zajmuje to podejście.</span><span class="sxs-lookup"><span data-stu-id="83a5a-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="83a5a-153">W ten sposób możesz **nie** mają identycznie nazwane kluczy w obu `<appSettings/>` i `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="83a5a-154">Użyj dwóch `EnvironmentConfigBuilder`s w `Greedy` trybu z prefiksami odrębne i `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="83a5a-155">Dzięki tej metodzie aplikacja może odczytywać `<appSettings/>` i `<connectionStrings/>` bez konieczności aktualizowania pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="83a5a-156">Następnej sekcji [stripPrefix](#stripprefix), pokazuje, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="83a5a-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="83a5a-157">Użyj dwóch `EnvironmentConfigBuilder`s w `Greedy` trybu z prefiksami distinct.</span><span class="sxs-lookup"><span data-stu-id="83a5a-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="83a5a-158">W przypadku tej metody nie może mieć zduplikowanych nazw kluczy zgodnie z nazwami kluczy muszą różnić się od prefiksu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="83a5a-159">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83a5a-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="83a5a-160">Ze znacznikami poprzedniego tego samego źródła prostego klucz/wartość może służyć do wypełniania konfiguracji dla dwóch różnych sekcji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="83a5a-161">Na poniższej ilustracji przedstawiono `<appSettings/>` i `<connectionStrings/>` kluczy/wartości z poprzednim *web.config* pliku zestawu w edytorze środowiska:</span><span class="sxs-lookup"><span data-stu-id="83a5a-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Edytor środowiska](config-builder/static/prefix.png)

<span data-ttu-id="83a5a-163">Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` kluczy/wartości zawartych w poprzednim *web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="83a5a-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="83a5a-164">Powyższy kod będzie równa wartości właściwości:</span><span class="sxs-lookup"><span data-stu-id="83a5a-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="83a5a-165">Wartości w *web.config* plik, jeśli klucze nie są ustawione w zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="83a5a-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="83a5a-166">Wartości środowiska zmiennej, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="83a5a-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="83a5a-167">Na przykład za pomocą poprzedniej *web.config* pliku, klucz/wartość w poprzednim obrazie edytora środowiska i poprzedniego kodu, następujące wartości są ustawione:</span><span class="sxs-lookup"><span data-stu-id="83a5a-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="83a5a-168">Key</span><span class="sxs-lookup"><span data-stu-id="83a5a-168">Key</span></span>              | <span data-ttu-id="83a5a-169">Wartość</span><span class="sxs-lookup"><span data-stu-id="83a5a-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="83a5a-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="83a5a-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="83a5a-171">AppSetting_ServiceID ze zmiennych env</span><span class="sxs-lookup"><span data-stu-id="83a5a-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="83a5a-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="83a5a-172">AppSetting_default</span></span>            | <span data-ttu-id="83a5a-173">Wartość AppSetting_default env</span><span class="sxs-lookup"><span data-stu-id="83a5a-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="83a5a-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="83a5a-174">ConnStr_default</span></span>         | <span data-ttu-id="83a5a-175">Val ConnStr_default z env</span><span class="sxs-lookup"><span data-stu-id="83a5a-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="83a5a-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="83a5a-176">stripPrefix</span></span>

<span data-ttu-id="83a5a-177">`stripPrefix`: atrybut typu wartość logiczna, wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="83a5a-178">Poprzedni znacznik XML oddziela ustawienia aplikacji z parametrów połączenia, ale wymaga wszystkie klucze w *web.config* pliku, aby użyć określonego prefiksu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="83a5a-179">Na przykład prefiks `AppSetting` muszą zostać dodane do `ServiceID` klucza ("AppSetting_ServiceID").</span><span class="sxs-lookup"><span data-stu-id="83a5a-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="83a5a-180">Za pomocą `stripPrefix`, prefiks nie jest używany w *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="83a5a-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="83a5a-181">Prefiks jest wymagany w źródle konstruktora konfiguracji (na przykład w środowisku.) Przewidujemy, że większość programistów użyje `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="83a5a-182">Aplikacje zazwyczaj paska wyłączone prefiks.</span><span class="sxs-lookup"><span data-stu-id="83a5a-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="83a5a-183">Następujące *web.config* paski prefiks:</span><span class="sxs-lookup"><span data-stu-id="83a5a-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="83a5a-184">W poprzednim *web.config* pliku `default` klucz znajduje się w obu `<appSettings/>` i `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="83a5a-185">Na poniższej ilustracji przedstawiono `<appSettings/>` i `<connectionStrings/>` kluczy/wartości z poprzednim *web.config* pliku zestawu w edytorze środowiska:</span><span class="sxs-lookup"><span data-stu-id="83a5a-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Edytor środowiska](config-builder/static/prefix.png)

<span data-ttu-id="83a5a-187">Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` kluczy/wartości zawartych w poprzednim *web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="83a5a-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="83a5a-188">Powyższy kod będzie równa wartości właściwości:</span><span class="sxs-lookup"><span data-stu-id="83a5a-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="83a5a-189">Wartości w *web.config* plik, jeśli klucze nie są ustawione w zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="83a5a-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="83a5a-190">Wartości środowiska zmiennej, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="83a5a-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="83a5a-191">Na przykład za pomocą poprzedniej *web.config* pliku, klucz/wartość w poprzednim obrazie edytora środowiska i poprzedniego kodu, następujące wartości są ustawione:</span><span class="sxs-lookup"><span data-stu-id="83a5a-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="83a5a-192">Key</span><span class="sxs-lookup"><span data-stu-id="83a5a-192">Key</span></span>              | <span data-ttu-id="83a5a-193">Wartość</span><span class="sxs-lookup"><span data-stu-id="83a5a-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="83a5a-194">Identyfikator usługi</span><span class="sxs-lookup"><span data-stu-id="83a5a-194">ServiceID</span></span>           | <span data-ttu-id="83a5a-195">AppSetting_ServiceID ze zmiennych env</span><span class="sxs-lookup"><span data-stu-id="83a5a-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="83a5a-196">default</span><span class="sxs-lookup"><span data-stu-id="83a5a-196">default</span></span>            | <span data-ttu-id="83a5a-197">Wartość AppSetting_default env</span><span class="sxs-lookup"><span data-stu-id="83a5a-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="83a5a-198">default</span><span class="sxs-lookup"><span data-stu-id="83a5a-198">default</span></span>         | <span data-ttu-id="83a5a-199">Val ConnStr_default z env</span><span class="sxs-lookup"><span data-stu-id="83a5a-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="83a5a-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="83a5a-200">tokenPattern</span></span>

<span data-ttu-id="83a5a-201">`tokenPattern`: Ciąg, wartość domyślna to `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="83a5a-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="83a5a-202">`Expand` Zachowanie Konstruktorzy wyszukuje nieprzetworzonym kodzie XML dla tokenów, które mają postać `${token}`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="83a5a-203">Wyszukiwanie jest przeprowadzane za pomocą wyrażeń regularnych domyślne `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="83a5a-204">Zestaw znaków, który odpowiada `\w` jest ściślejsze niż XML i wielu źródeł w konfiguracji jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="83a5a-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="83a5a-205">Użyj `tokenPattern` po więcej znaków niż `@"\$\{(\w+)\}"` wymagane są nazwa tokenu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="83a5a-206">`tokenPattern`: Ciąg:</span><span class="sxs-lookup"><span data-stu-id="83a5a-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="83a5a-207">Umożliwia deweloperom Zmienianie wyrażenie regularne, używany do dopasowywania tokenu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="83a5a-208">Aby upewnić się, czy jest poprawnie sformułowanym nieszkodliwe wyrażenie regularne jest wykonywana żadna Weryfikacja.</span><span class="sxs-lookup"><span data-stu-id="83a5a-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="83a5a-209">Musi zawierać grupę przechwytywania.</span><span class="sxs-lookup"><span data-stu-id="83a5a-209">It must contain a capture group.</span></span> <span data-ttu-id="83a5a-210">Całe wyrażenie regularne musi odpowiadać całego tokenu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="83a5a-211">Pierwszy przechwytywania musi być nazwa tokenu do przeszukania źródło konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="83a5a-212">Konstruktorzy konfiguracji w Microsoft.Configuration.ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="83a5a-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="83a5a-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="83a5a-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="83a5a-214">[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="83a5a-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="83a5a-215">To najprostsza z konstruktorów konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="83a5a-216">Odczytuje wartości ze środowiska.</span><span class="sxs-lookup"><span data-stu-id="83a5a-216">Reads values from the environment.</span></span>
* <span data-ttu-id="83a5a-217">Nie ma żadnych dodatkowych opcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="83a5a-218">`name` Wartość atrybutu jest określana.</span><span class="sxs-lookup"><span data-stu-id="83a5a-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="83a5a-219">**Uwaga:** w środowisku Windows container zmienne ustawione w czasie wykonywania są wprowadzane tylko w środowisku procesu punktu wejścia.</span><span class="sxs-lookup"><span data-stu-id="83a5a-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="83a5a-220">Aplikacje działają jako usługę lub proces bez punktu wejścia nie wybierze tych zmiennych, chyba że w przeciwnym razie są one wprowadzane za pośrednictwem mechanizmu w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="83a5a-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="83a5a-221">Dla [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)— na podstawie kontenerów, bieżąca wersja [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) obsługuje w *DefaultAppPool* tylko.</span><span class="sxs-lookup"><span data-stu-id="83a5a-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="83a5a-222">Inne odmiany kontenera z systemem Windows może być konieczne opracowanie własnych zasad iniekcji dla procesów bez punktu wejścia.</span><span class="sxs-lookup"><span data-stu-id="83a5a-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="83a5a-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="83a5a-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="83a5a-224">Nigdy nie przechowywanie haseł, parametrów połączenia poufne lub innych danych poufnych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="83a5a-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="83a5a-225">Wpisy tajne w środowisku produkcyjnym nie powinny być używane do projektowania lub testowania.</span><span class="sxs-lookup"><span data-stu-id="83a5a-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="83a5a-226">Ten konstruktor konfiguracji udostępnia funkcję podobne do [platformy ASP.NET Core klucz tajny Menedżera](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="83a5a-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="83a5a-227">[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) mogą być używane w projektach .NET Framework, ale musi zostać określony plik wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="83a5a-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="83a5a-228">Alternatywnie można zdefiniować `UserSecretsId` właściwość w projekcie plików, a następnie utwórz plik raw wpisów tajnych w bieżącej lokalizacji do odczytu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="83a5a-229">Aby zachować zależności zewnętrznych spoza projektu, plik wpisów tajnych jest formacie XML.</span><span class="sxs-lookup"><span data-stu-id="83a5a-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="83a5a-230">Formatowanie, XML jest szczegółowo opisuje implementacja i format nie powinna być używana na.</span><span class="sxs-lookup"><span data-stu-id="83a5a-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="83a5a-231">Jeśli zachodzi potrzeba udostępniania *secrets.json* plików w projektach .NET Core, należy rozważyć użycie [SimpleJsonConfigBuilder](#simplejsonconfig).</span><span class="sxs-lookup"><span data-stu-id="83a5a-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfig).</span></span> <span data-ttu-id="83a5a-232">`SimpleJsonConfigBuilder` Formatowania dla platformy .NET Core należy również uwzględnić szczegółowo opisuje implementacja może ulec zmianie.</span><span class="sxs-lookup"><span data-stu-id="83a5a-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="83a5a-233">Konfiguracja atrybuty dla `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="83a5a-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="83a5a-234">`userSecretsId` — Jest preferowana metoda identyfikowania plik wpisów tajnych XML.</span><span class="sxs-lookup"><span data-stu-id="83a5a-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="83a5a-235">Działa podobnie do platformy .NET Core, która używa `UserSecretsId` właściwość do przechowywania tego identyfikatora projektu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="83a5a-236">Ciąg musi być unikatowy, nie musi być identyfikatorem GUID.</span><span class="sxs-lookup"><span data-stu-id="83a5a-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="83a5a-237">Z tym atrybutem `UserSecretsConfigBuilder` przeszukania znaną lokalizacją lokalną (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) dla pliku wpisów tajnych należących do tego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="83a5a-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="83a5a-238">`userSecretsFile` -Opcjonalny atrybut, określając pliku zawierającego wpisy tajne.</span><span class="sxs-lookup"><span data-stu-id="83a5a-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="83a5a-239">`~` Znak może być użyty na początku, aby odwoływać się do katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="83a5a-240">Albo tego atrybutu lub `userSecretsId` atrybut jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="83a5a-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="83a5a-241">Jeśli są określone oba `userSecretsFile` ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="83a5a-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="83a5a-242">`optional`: wartość logiczna, wartość domyślna `true` — uniemożliwia wyjątek, jeśli nie można odnaleźć pliku wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="83a5a-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="83a5a-243">`name` Wartość atrybutu jest określana.</span><span class="sxs-lookup"><span data-stu-id="83a5a-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="83a5a-244">Plik wpisów tajnych ma następujący format:</span><span class="sxs-lookup"><span data-stu-id="83a5a-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="83a5a-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="83a5a-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="83a5a-246">[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) odczytuje wartości przechowywane w [usługi Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="83a5a-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="83a5a-247">`vaultName` jest wymagana (nazwę magazynu) lub identyfikator URI do magazynu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="83a5a-248">Inne atrybuty umożliwiają kontrolę, o których Magazyn, aby nawiązać połączenie, ale są wymagane tylko, jeśli aplikacja nie jest uruchomiona w środowisku, które współpracuje z `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="83a5a-249">Biblioteki uwierzytelniania usługi Azure umożliwia automatyczne pobranie, jeśli jest to możliwe informacji o połączeniu ze środowiska wykonawczego.</span><span class="sxs-lookup"><span data-stu-id="83a5a-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="83a5a-250">Możesz zastąpić automatycznie wybierze informacji o połączeniu, podając parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="83a5a-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="83a5a-251">`vaultName` -Jeśli wymagane `uri` w nie podano.</span><span class="sxs-lookup"><span data-stu-id="83a5a-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="83a5a-252">Określa nazwę magazynu, w ramach subskrypcji platformy Azure, z którego można odczytać pary klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="83a5a-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="83a5a-253">`connectionString` -Parametry połączenia mogą być używane przez [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="83a5a-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="83a5a-254">`uri` -Łączy się z innymi dostawcami usługi Key Vault przy użyciu określonego `uri` wartość.</span><span class="sxs-lookup"><span data-stu-id="83a5a-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="83a5a-255">Jeśli nie zostanie określony, Azure (`vaultName`) jest dostawcą magazynu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="83a5a-256">`version` — Usługa azure Key Vault zapewnia funkcji przechowywania wersji dla wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="83a5a-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="83a5a-257">Jeśli `version` określono konstruktora pobiera tylko wpisy tajne dopasowania tej wersji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="83a5a-258">`preloadSecretNames` -Domyślnie querys tego konstruktora **wszystkich** klucza nazwy w usłudze key vault, po jego zainicjowaniu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="83a5a-259">Aby zapobiec, odczytywanie wszystkich wartości kluczy, ustaw ten atrybut na `false`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="83a5a-260">Ustawienie tej opcji na `false` odczytuje wpisy tajne jednego naraz.</span><span class="sxs-lookup"><span data-stu-id="83a5a-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="83a5a-261">Odczytywanie wpisów tajnych, które można pojedynczo przydatne, jeśli magazyn zezwala na dostęp "Pobierz", ale nie "List" dostęp do.</span><span class="sxs-lookup"><span data-stu-id="83a5a-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="83a5a-262">**Uwaga:** przy użyciu `Greedy` trybie `preloadSecretNames` musi być `true` (ustawienie domyślne).</span><span class="sxs-lookup"><span data-stu-id="83a5a-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="83a5a-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="83a5a-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="83a5a-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) jest konstruktor podstawowej konfiguracji, który korzysta z plików w katalogu jako źródła wartości.</span><span class="sxs-lookup"><span data-stu-id="83a5a-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="83a5a-265">Nazwa pliku jest klucz, a zawartość jest wartość.</span><span class="sxs-lookup"><span data-stu-id="83a5a-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="83a5a-266">Tego konstruktora konfiguracji może być przydatne podczas pracy w środowisku kontenera zorganizowane.</span><span class="sxs-lookup"><span data-stu-id="83a5a-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="83a5a-267">Systemy, takie jak Docker Swarm i Kubernetes zapewniają `secrets` do ich kontenerów windows zorganizowane w ten sposób klucza każdego pliku.</span><span class="sxs-lookup"><span data-stu-id="83a5a-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="83a5a-268">Szczegóły atrybutu:</span><span class="sxs-lookup"><span data-stu-id="83a5a-268">Attribute details:</span></span>

* <span data-ttu-id="83a5a-269">`directoryPath` — Wymagane.</span><span class="sxs-lookup"><span data-stu-id="83a5a-269">`directoryPath` - Required.</span></span> <span data-ttu-id="83a5a-270">Określa ścieżkę do wyszukania wartości.</span><span class="sxs-lookup"><span data-stu-id="83a5a-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="83a5a-271">Docker for Windows klucze tajne są przechowywane w *C:\ProgramData\Docker\secrets* katalogu domyślnie.</span><span class="sxs-lookup"><span data-stu-id="83a5a-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="83a5a-272">`ignorePrefix` -Pliki, rozpoczynające się od tego prefiksu są wyłączone.</span><span class="sxs-lookup"><span data-stu-id="83a5a-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="83a5a-273">Wartość domyślna to "ignorowania".</span><span class="sxs-lookup"><span data-stu-id="83a5a-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="83a5a-274">`keyDelimiter` — Wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="83a5a-275">Jeśli zostanie określony, Konstruktor konfiguracji przechodzi przez wiele poziomów katalogu, tworzenia nazw kluczy, z tym ogranicznikiem.</span><span class="sxs-lookup"><span data-stu-id="83a5a-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="83a5a-276">Jeśli ta wartość jest `null`, Konstruktor konfiguracji przeszukuje tylko na najwyższym poziomie w katalogu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="83a5a-277">`optional` — Wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="83a5a-278">Określa, czy konstruktora konfiguracji powinna powodować błędy, jeśli katalog źródłowy nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="83a5a-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="83a5a-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="83a5a-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="83a5a-280">Nigdy nie przechowywanie haseł, parametrów połączenia poufne lub innych danych poufnych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="83a5a-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="83a5a-281">Wpisy tajne w środowisku produkcyjnym nie powinny być używane do projektowania lub testowania.</span><span class="sxs-lookup"><span data-stu-id="83a5a-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="83a5a-282">Projekty .NET core często używane pliki w formacie JSON dla konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="83a5a-283">[SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) konstruktora umożliwia plików .NET Core w formacie JSON ma być używany w programie .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="83a5a-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="83a5a-284">Ten konstruktor konfiguracji jest zapewnia podstawowe mapowanie ze źródła prostego klucz/wartość w obszarach określonego klucza i wartości konfiguracji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="83a5a-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="83a5a-285">Jest to Konstruktor konfiguracji **nie** zapewnienia konfiguracji hierarchicznej.</span><span class="sxs-lookup"><span data-stu-id="83a5a-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="83a5a-286">Tworzenie kopii pliku JSON jest podobny do słownika nie hierarchiczny obiekt złożony.</span><span class="sxs-lookup"><span data-stu-id="83a5a-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="83a5a-287">Można użyć wielopoziomowe, hierarchiczne pliku.</span><span class="sxs-lookup"><span data-stu-id="83a5a-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="83a5a-288">Ten dostawca `flatten`s głębi, dodając nazwy właściwości na każdym poziomie przy użyciu `:` jako ogranicznik.</span><span class="sxs-lookup"><span data-stu-id="83a5a-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="83a5a-289">Szczegóły atrybutu:</span><span class="sxs-lookup"><span data-stu-id="83a5a-289">Attribute details:</span></span>

* <span data-ttu-id="83a5a-290">`jsonFile` — Wymagane.</span><span class="sxs-lookup"><span data-stu-id="83a5a-290">`jsonFile` - Required.</span></span> <span data-ttu-id="83a5a-291">Określa plik JSON do odczytu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="83a5a-292">`~` Znak może być użyty na początku, aby odwoływać się do katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="83a5a-293">`optional` — Wartość logiczna, wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="83a5a-294">Zapobiega zgłaszanie wyjątków, jeśli nie można odnaleźć pliku JSON.</span><span class="sxs-lookup"><span data-stu-id="83a5a-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="83a5a-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="83a5a-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="83a5a-296">`Flat` jest ustawieniem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="83a5a-296">`Flat` is the default.</span></span> <span data-ttu-id="83a5a-297">Gdy `jsonMode` jest `Flat`, plik JSON jest źródłem jednego prostego klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="83a5a-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="83a5a-298">`EnvironmentConfigBuilder` i `AzureKeyVaultConfigBuilder` są również źródeł jednego prostego klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="83a5a-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="83a5a-299">Gdy `SimpleJsonConfigBuilder` jest skonfigurowana w `Sectional` trybu:</span><span class="sxs-lookup"><span data-stu-id="83a5a-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="83a5a-300">Plik JSON jest koncepcyjnie podzielony tylko na najwyższym poziomie na wiele słowników.</span><span class="sxs-lookup"><span data-stu-id="83a5a-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="83a5a-301">Każdy słowniki są stosowane tylko do sekcji konfiguracji, który pasuje do nazwy właściwości najwyższego poziomu do nich dołączone.</span><span class="sxs-lookup"><span data-stu-id="83a5a-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="83a5a-302">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="83a5a-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="83a5a-303">Implementowanie konstruktora niestandardowego klucza i wartości konfiguracji</span><span class="sxs-lookup"><span data-stu-id="83a5a-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="83a5a-304">Jeśli Konstruktorzy konfiguracji nie odpowiada Twoim potrzebom, możesz napisać niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="83a5a-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="83a5a-305">`KeyValueConfigBuilder` Klasy bazowej obsługuje tryby podstawienia i większości problemów prefiks.</span><span class="sxs-lookup"><span data-stu-id="83a5a-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="83a5a-306">Implementowanie projektu potrzebne tylko:</span><span class="sxs-lookup"><span data-stu-id="83a5a-306">An implementing project need only:</span></span>

* <span data-ttu-id="83a5a-307">Dziedziczenie z klasy bazowej i wdrożenie podstawowego źródła par klucz/wartość za pośrednictwem `GetValue` i `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="83a5a-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="83a5a-308">Dodaj [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) do projektu.</span><span class="sxs-lookup"><span data-stu-id="83a5a-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="83a5a-309">`KeyValueConfigBuilder` Klasy bazowej zapewnia znaczną część pracy i spójne zachowanie różnych kluczy/wartości Konstruktorzy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="83a5a-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="83a5a-310">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="83a5a-310">Additional resources</span></span>

* [<span data-ttu-id="83a5a-311">Repozytorium GitHub Konstruktorzy konfiguracji</span><span class="sxs-lookup"><span data-stu-id="83a5a-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="83a5a-312">Usługa Usługa uwierzytelniania usługi Azure Key Vault przy użyciu platformy .NET</span><span class="sxs-lookup"><span data-stu-id="83a5a-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
