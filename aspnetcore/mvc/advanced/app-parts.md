---
title: Części aplikacji w ASP.NET Core
author: rick-anderson
description: Udostępnianie kontrolerów, przeglądanie, Razor Pages i nie tylko za pomocą części aplikacji w ASP.NET Core
ms.author: riande
ms.date: 11/7/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: ff6afa1852a3ee97fc4dbbae970dd746ec92f74c
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799465"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="3aa6d-103">Udostępnianie kontrolerów, widoków, Razor Pages i innych elementów aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3aa6d-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3aa6d-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3aa6d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3aa6d-105">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3aa6d-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3aa6d-106">*Część aplikacji* to Abstrakcja zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="3aa6d-107">Części aplikacji umożliwiają ASP.NET Core odnajdywania kontrolerów, wyświetlania składników, pomocników tagów, Razor Pages, źródeł kompilacji Razor i nie tylko.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="3aa6d-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) jest częścią aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="3aa6d-109">`AssemblyPart` hermetyzuje odwołanie do zestawu i udostępnia typy i odwołania do kompilacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="3aa6d-110">*Dostawcy funkcji* pracują z częściami aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="3aa6d-111">Głównym przypadkiem użycia części aplikacji jest skonfigurowanie aplikacji do odnajdywania (lub unikania ładowania) ASP.NET Core funkcji z zestawu.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="3aa6d-112">Na przykład może być konieczne udostępnienie typowych funkcji między wieloma aplikacjami.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="3aa6d-113">Korzystając z części aplikacji, można udostępnić zestaw (DLL) zawierający kontrolery, widoki, Razor Pages, źródła kompilacji Razor, pomocników tagów i wiele innych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="3aa6d-114">Udostępnianie zestawu jest preferowane do duplikowania kodu w wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="3aa6d-115">ASP.NET Core aplikacje ładują funkcje z <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="3aa6d-116">Klasa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> reprezentuje część aplikacji, która jest obsługiwana przez zestaw.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="3aa6d-117">Załaduj funkcje ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3aa6d-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="3aa6d-118">Użyj klas `ApplicationPart` i `AssemblyPart` do odnajdywania i ładowania funkcji ASP.NET Core (kontrolerów, składników widoku itp.).</span><span class="sxs-lookup"><span data-stu-id="3aa6d-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="3aa6d-119"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> śledzi dostępne składniki aplikacji i dostawców funkcji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="3aa6d-120">`ApplicationPartManager` jest skonfigurowany w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="3aa6d-121">Poniższy kod stanowi alternatywny sposób konfigurowania `ApplicationPartManager` przy użyciu `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="3aa6d-122">Powyższe dwa przykłady kodu ładują `SharedController` z zestawu.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="3aa6d-123">`SharedController` nie znajduje się w projekcie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-123">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="3aa6d-124">Zobacz Pobieranie przykładowego [rozwiązania WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="3aa6d-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="3aa6d-125">Dołącz widoki</span><span class="sxs-lookup"><span data-stu-id="3aa6d-125">Include views</span></span>

<span data-ttu-id="3aa6d-126">Aby uwzględnić widoki w zestawie:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-126">To include views in the assembly:</span></span>

* <span data-ttu-id="3aa6d-127">Dodaj następujący znacznik do pliku projektu udostępnionego:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-127">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="3aa6d-128">Dodaj <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> do <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="3aa6d-129">Zapobiegaj ładowaniu zasobów</span><span class="sxs-lookup"><span data-stu-id="3aa6d-129">Prevent loading resources</span></span>

<span data-ttu-id="3aa6d-130">Części aplikacji mogą służyć do *uniknięcia* ładowania zasobów w określonym zestawie lub lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="3aa6d-131">Dodaj lub Usuń elementy członkowskie kolekcji <xref:Microsoft.AspNetCore.Mvc.ApplicationParts>, aby ukryć lub udostępnić dostępne zasoby.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="3aa6d-132">Kolejność wpisów w kolekcji `ApplicationParts` nie jest ważna.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="3aa6d-133">Skonfiguruj `ApplicationPartManager` przed użyciem go do konfigurowania usług w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="3aa6d-134">Na przykład skonfiguruj `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="3aa6d-135">Wywołaj `Remove` w kolekcji `ApplicationParts`, aby usunąć zasób.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="3aa6d-136">Poniższy kod używa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> do usuwania `MyDependentLibrary` z aplikacji: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="3aa6d-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="3aa6d-137">`ApplicationPartManager` zawiera części dla:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="3aa6d-138">Zestaw aplikacji i zestawy zależne.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-138">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="3aa6d-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.,</span><span class="sxs-lookup"><span data-stu-id="3aa6d-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="3aa6d-140">`Microsoft.AspNetCore.Mvc.Razor`.,</span><span class="sxs-lookup"><span data-stu-id="3aa6d-140">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="3aa6d-141">Dostawcy funkcji aplikacji</span><span class="sxs-lookup"><span data-stu-id="3aa6d-141">Application feature providers</span></span>

<span data-ttu-id="3aa6d-142">Dostawcy funkcji aplikacji badają części aplikacji i udostępniają funkcje dla tych części.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-142">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="3aa6d-143">Istnieją Wbudowani dostawcy funkcji dla następujących funkcji ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-143">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="3aa6d-144">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="3aa6d-144">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="3aa6d-145">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="3aa6d-145">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="3aa6d-146">Wyświetl składniki</span><span class="sxs-lookup"><span data-stu-id="3aa6d-146">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="3aa6d-147">Dostawcy funkcji dziedziczą z <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, gdzie `T` jest typem funkcji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-147">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="3aa6d-148">Dostawców funkcji można zaimplementować dla dowolnego z wcześniej wymienionych typów funkcji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-148">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="3aa6d-149">Kolejność dostawców funkcji w `ApplicationPartManager.FeatureProviders` może mieć wpływ na zachowanie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-149">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="3aa6d-150">Później dodani dostawcy mogą reagować na akcje podejmowane przez wcześniej dodanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-150">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="3aa6d-151">Funkcja kontrolera ogólnego</span><span class="sxs-lookup"><span data-stu-id="3aa6d-151">Generic controller feature</span></span>

<span data-ttu-id="3aa6d-152">ASP.NET Core ignoruje [Ogólne kontrolery](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="3aa6d-152">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="3aa6d-153">Kontroler generyczny ma parametr typu (na przykład `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="3aa6d-153">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="3aa6d-154">Poniższy przykład dodaje wystąpienia kontrolera ogólnego dla określonej listy typów:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-154">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="3aa6d-155">Typy są zdefiniowane w `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-155">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="3aa6d-156">Dostawca funkcji jest dodawany w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-156">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="3aa6d-157">Nazwy kontrolerów ogólnych używane do routingu mają postać *GenericController ' 1 [widget]* , a nie *widżetu*.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-157">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="3aa6d-158">Następujący atrybut modyfikuje nazwę odpowiadającą typowi ogólnemu używanemu przez kontroler:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-158">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="3aa6d-159">Klasa `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-159">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="3aa6d-160">Na przykład żądanie adresu URL `https://localhost:5001/Sprocket` powoduje następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-160">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="3aa6d-161">Wyświetlanie dostępnych funkcji</span><span class="sxs-lookup"><span data-stu-id="3aa6d-161">Display available features</span></span>

<span data-ttu-id="3aa6d-162">Funkcje dostępne dla aplikacji można wyliczyć, żądając `ApplicationPartManager` za pomocą [iniekcji zależności](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="3aa6d-162">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="3aa6d-163">[Przykład pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) używa powyższego kodu, aby wyświetlić funkcje aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-163">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

::: moniker-end

::: moniker range="<= aspnetcore-3.0"

<span data-ttu-id="3aa6d-164">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3aa6d-164">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3aa6d-165">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3aa6d-165">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3aa6d-166">*Część aplikacji* to Abstrakcja zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-166">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="3aa6d-167">Części aplikacji umożliwiają ASP.NET Core odnajdywania kontrolerów, wyświetlania składników, pomocników tagów, Razor Pages, źródeł kompilacji Razor i nie tylko.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-167">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="3aa6d-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) jest częścią aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="3aa6d-169">`AssemblyPart` hermetyzuje odwołanie do zestawu i udostępnia typy i odwołania do kompilacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-169">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="3aa6d-170">*Dostawcy funkcji* pracują z częściami aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-170">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="3aa6d-171">Głównym przypadkiem użycia części aplikacji jest skonfigurowanie aplikacji do odnajdywania (lub unikania ładowania) ASP.NET Core funkcji z zestawu.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-171">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="3aa6d-172">Na przykład może być konieczne udostępnienie typowych funkcji między wieloma aplikacjami.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-172">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="3aa6d-173">Korzystając z części aplikacji, można udostępnić zestaw (DLL) zawierający kontrolery, widoki, Razor Pages, źródła kompilacji Razor, pomocników tagów i wiele innych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-173">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="3aa6d-174">Udostępnianie zestawu jest preferowane do duplikowania kodu w wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-174">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="3aa6d-175">ASP.NET Core aplikacje ładują funkcje z <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-175">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="3aa6d-176">Klasa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> reprezentuje część aplikacji, która jest obsługiwana przez zestaw.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-176">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="3aa6d-177">Załaduj funkcje ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3aa6d-177">Load ASP.NET Core features</span></span>

<span data-ttu-id="3aa6d-178">Użyj klas `ApplicationPart` i `AssemblyPart` do odnajdywania i ładowania funkcji ASP.NET Core (kontrolerów, składników widoku itp.).</span><span class="sxs-lookup"><span data-stu-id="3aa6d-178">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="3aa6d-179"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> śledzi dostępne składniki aplikacji i dostawców funkcji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-179">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="3aa6d-180">`ApplicationPartManager` jest skonfigurowany w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-180">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="3aa6d-181">Poniższy kod stanowi alternatywny sposób konfigurowania `ApplicationPartManager` przy użyciu `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-181">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="3aa6d-182">Powyższe dwa przykłady kodu ładują `SharedController` z zestawu.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-182">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="3aa6d-183">`SharedController` nie znajduje się w projekcie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-183">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="3aa6d-184">Zobacz Pobieranie przykładowego [rozwiązania WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="3aa6d-184">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="3aa6d-185">Dołącz widoki</span><span class="sxs-lookup"><span data-stu-id="3aa6d-185">Include views</span></span>

<span data-ttu-id="3aa6d-186">Aby uwzględnić widoki w zestawie:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-186">To include views in the assembly:</span></span>

* <span data-ttu-id="3aa6d-187">Dodaj następujący znacznik do pliku projektu udostępnionego:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-187">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="3aa6d-188">Dodaj <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> do <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-188">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="3aa6d-189">Zapobiegaj ładowaniu zasobów</span><span class="sxs-lookup"><span data-stu-id="3aa6d-189">Prevent loading resources</span></span>

<span data-ttu-id="3aa6d-190">Części aplikacji mogą służyć do *uniknięcia* ładowania zasobów w określonym zestawie lub lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-190">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="3aa6d-191">Dodaj lub Usuń elementy członkowskie kolekcji <xref:Microsoft.AspNetCore.Mvc.ApplicationParts>, aby ukryć lub udostępnić dostępne zasoby.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-191">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="3aa6d-192">Kolejność wpisów w kolekcji `ApplicationParts` nie jest ważna.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-192">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="3aa6d-193">Skonfiguruj `ApplicationPartManager` przed użyciem go do konfigurowania usług w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-193">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="3aa6d-194">Na przykład skonfiguruj `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-194">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="3aa6d-195">Wywołaj `Remove` w kolekcji `ApplicationParts`, aby usunąć zasób.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-195">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="3aa6d-196">Poniższy kod używa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> do usuwania `MyDependentLibrary` z aplikacji: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="3aa6d-196">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="3aa6d-197">`ApplicationPartManager` zawiera części dla:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-197">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="3aa6d-198">Zestaw aplikacji i zestawy zależne.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-198">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="3aa6d-199">`Microsoft.AspNetCore.Mvc.TagHelpers`.,</span><span class="sxs-lookup"><span data-stu-id="3aa6d-199">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="3aa6d-200">`Microsoft.AspNetCore.Mvc.Razor`.,</span><span class="sxs-lookup"><span data-stu-id="3aa6d-200">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="3aa6d-201">Dostawcy funkcji aplikacji</span><span class="sxs-lookup"><span data-stu-id="3aa6d-201">Application feature providers</span></span>

<span data-ttu-id="3aa6d-202">Dostawcy funkcji aplikacji badają części aplikacji i udostępniają funkcje dla tych części.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-202">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="3aa6d-203">Istnieją Wbudowani dostawcy funkcji dla następujących funkcji ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-203">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="3aa6d-204">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="3aa6d-204">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="3aa6d-205">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="3aa6d-205">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="3aa6d-206">Wyświetl składniki</span><span class="sxs-lookup"><span data-stu-id="3aa6d-206">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="3aa6d-207">Dostawcy funkcji dziedziczą z <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, gdzie `T` jest typem funkcji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-207">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="3aa6d-208">Dostawców funkcji można zaimplementować dla dowolnego z wcześniej wymienionych typów funkcji.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-208">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="3aa6d-209">Kolejność dostawców funkcji w `ApplicationPartManager.FeatureProviders` może mieć wpływ na zachowanie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-209">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="3aa6d-210">Później dodani dostawcy mogą reagować na akcje podejmowane przez wcześniej dodanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-210">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="3aa6d-211">Funkcja kontrolera ogólnego</span><span class="sxs-lookup"><span data-stu-id="3aa6d-211">Generic controller feature</span></span>

<span data-ttu-id="3aa6d-212">ASP.NET Core ignoruje [Ogólne kontrolery](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="3aa6d-212">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="3aa6d-213">Kontroler generyczny ma parametr typu (na przykład `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="3aa6d-213">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="3aa6d-214">Poniższy przykład dodaje wystąpienia kontrolera ogólnego dla określonej listy typów:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-214">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="3aa6d-215">Typy są zdefiniowane w `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-215">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="3aa6d-216">Dostawca funkcji jest dodawany w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-216">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="3aa6d-217">Nazwy kontrolerów ogólnych używane do routingu mają postać *GenericController ' 1 [widget]* , a nie *widżetu*.</span><span class="sxs-lookup"><span data-stu-id="3aa6d-217">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="3aa6d-218">Następujący atrybut modyfikuje nazwę odpowiadającą typowi ogólnemu używanemu przez kontroler:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-218">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="3aa6d-219">Klasa `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-219">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="3aa6d-220">Na przykład żądanie adresu URL `https://localhost:5001/Sprocket` powoduje następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-220">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="3aa6d-221">Wyświetlanie dostępnych funkcji</span><span class="sxs-lookup"><span data-stu-id="3aa6d-221">Display available features</span></span>

<span data-ttu-id="3aa6d-222">Funkcje dostępne dla aplikacji można wyliczyć, żądając `ApplicationPartManager` za pomocą [iniekcji zależności](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="3aa6d-222">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="3aa6d-223">[Przykład pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) używa powyższego kodu, aby wyświetlić funkcje aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3aa6d-223">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

::: moniker-end