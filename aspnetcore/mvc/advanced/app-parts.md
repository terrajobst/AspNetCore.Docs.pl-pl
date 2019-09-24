---
title: Części aplikacji w ASP.NET Core
author: rick-anderson
description: Udostępnianie kontrolerów, przeglądanie, Razor Pages i nie tylko za pomocą części aplikacji w ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4b4c8c554a7045a180b56cf9998ab1a8496cde1b
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207351"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="ac534-103">Udostępnianie kontrolerów, widoków, Razor Pages i innych elementów aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac534-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="ac534-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ac534-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ac534-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ac534-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ac534-106">*Część aplikacji* to Abstrakcja zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ac534-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="ac534-107">Części aplikacji umożliwiają ASP.NET Core odnajdywania kontrolerów, wyświetlania składników, pomocników tagów, Razor Pages, źródeł kompilacji Razor i nie tylko.</span><span class="sxs-lookup"><span data-stu-id="ac534-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="ac534-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) jest częścią aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ac534-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="ac534-109">`AssemblyPart`hermetyzuje odwołanie do zestawu i udostępnia typy i odwołania do kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ac534-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="ac534-110">*Dostawcy funkcji* pracują z częściami aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac534-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="ac534-111">Głównym przypadkiem użycia części aplikacji jest skonfigurowanie aplikacji do odnajdywania (lub unikania ładowania) ASP.NET Core funkcji z zestawu.</span><span class="sxs-lookup"><span data-stu-id="ac534-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="ac534-112">Na przykład może być konieczne udostępnienie typowych funkcji między wieloma aplikacjami.</span><span class="sxs-lookup"><span data-stu-id="ac534-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="ac534-113">Korzystając z części aplikacji, można udostępnić zestaw (DLL) zawierający kontrolery, widoki, Razor Pages, źródła kompilacji Razor, pomocników tagów i wiele innych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ac534-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="ac534-114">Udostępnianie zestawu jest preferowane do duplikowania kodu w wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="ac534-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="ac534-115">ASP.NET Core aplikacje ładują funkcje <xref:System.Web.WebPages.ApplicationPart>z programu.</span><span class="sxs-lookup"><span data-stu-id="ac534-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="ac534-116"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> Klasa reprezentuje część aplikacji, która jest obsługiwana przez zestaw.</span><span class="sxs-lookup"><span data-stu-id="ac534-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="ac534-117">Załaduj funkcje ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac534-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="ac534-118">Użyj klas `AssemblyPart` i, aby odnajdywać i ładować funkcje ASP.NET Core (kontrolery, składniki widoku itp.). `ApplicationPart`</span><span class="sxs-lookup"><span data-stu-id="ac534-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="ac534-119"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> Śledzi dostępne części aplikacji i dostawców funkcji.</span><span class="sxs-lookup"><span data-stu-id="ac534-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="ac534-120">`ApplicationPartManager`jest skonfigurowany w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ac534-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="ac534-121">Poniższy kod stanowi alternatywny sposób konfigurowania `ApplicationPartManager` użycia: `AssemblyPart`</span><span class="sxs-lookup"><span data-stu-id="ac534-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="ac534-122">Powyższe dwa przykłady kodu ładują `SharedController` z zestawu.</span><span class="sxs-lookup"><span data-stu-id="ac534-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="ac534-123">Nie `SharedController` znajduje się w projekcie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ac534-123">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="ac534-124">Zobacz Pobieranie przykładowego [rozwiązania WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="ac534-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="ac534-125">Dołącz widoki</span><span class="sxs-lookup"><span data-stu-id="ac534-125">Include views</span></span>

<span data-ttu-id="ac534-126">Aby uwzględnić widoki w zestawie:</span><span class="sxs-lookup"><span data-stu-id="ac534-126">To include views in the assembly:</span></span>

* <span data-ttu-id="ac534-127">Dodaj następujący znacznik do pliku projektu udostępnionego:</span><span class="sxs-lookup"><span data-stu-id="ac534-127">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="ac534-128"><xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> Dodaj<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>do:</span><span class="sxs-lookup"><span data-stu-id="ac534-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="ac534-129">Zapobiegaj ładowaniu zasobów</span><span class="sxs-lookup"><span data-stu-id="ac534-129">Prevent loading resources</span></span>

<span data-ttu-id="ac534-130">Części aplikacji mogą służyć do *uniknięcia* ładowania zasobów w określonym zestawie lub lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="ac534-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="ac534-131">Dodaj lub Usuń elementy członkowskie <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> kolekcji, aby ukryć lub udostępnić dostępne zasoby.</span><span class="sxs-lookup"><span data-stu-id="ac534-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="ac534-132">Kolejność wpisów w `ApplicationParts` kolekcji nie jest ważna.</span><span class="sxs-lookup"><span data-stu-id="ac534-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="ac534-133">Skonfiguruj program `ApplicationPartManager` przed użyciem go do konfigurowania usług w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="ac534-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="ac534-134">Na przykład skonfiguruj `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="ac534-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="ac534-135">Wywołaj `Remove`kolekcję,abyusunąć zasób.`ApplicationParts`</span><span class="sxs-lookup"><span data-stu-id="ac534-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="ac534-136">Następujący kod używa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> do usunięcia `MyDependentLibrary` z aplikacji:[!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="ac534-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="ac534-137">`ApplicationPartManager` Zawiera części dla:</span><span class="sxs-lookup"><span data-stu-id="ac534-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="ac534-138">Zestaw aplikacji i zestawy zależne.</span><span class="sxs-lookup"><span data-stu-id="ac534-138">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="ac534-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="ac534-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="ac534-140">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="ac534-140">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="ac534-141">Dostawcy funkcji aplikacji</span><span class="sxs-lookup"><span data-stu-id="ac534-141">Application feature providers</span></span>

<span data-ttu-id="ac534-142">Dostawcy funkcji aplikacji badają części aplikacji i udostępniają funkcje dla tych części.</span><span class="sxs-lookup"><span data-stu-id="ac534-142">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="ac534-143">Istnieją Wbudowani dostawcy funkcji dla następujących funkcji ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="ac534-143">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="ac534-144">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="ac534-144">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="ac534-145">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="ac534-145">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="ac534-146">Wyświetl składniki</span><span class="sxs-lookup"><span data-stu-id="ac534-146">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="ac534-147">Dostawcy funkcji dziedziczą <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>z, `T` gdzie jest typem funkcji.</span><span class="sxs-lookup"><span data-stu-id="ac534-147">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="ac534-148">Dostawców funkcji można zaimplementować dla dowolnego z wcześniej wymienionych typów funkcji.</span><span class="sxs-lookup"><span data-stu-id="ac534-148">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="ac534-149">Kolejność dostawców funkcji w programie `ApplicationPartManager.FeatureProviders` może mieć wpływ na zachowanie.</span><span class="sxs-lookup"><span data-stu-id="ac534-149">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="ac534-150">Później dodani dostawcy mogą reagować na akcje podejmowane przez wcześniej dodanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="ac534-150">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="ac534-151">Funkcja kontrolera ogólnego</span><span class="sxs-lookup"><span data-stu-id="ac534-151">Generic controller feature</span></span>

<span data-ttu-id="ac534-152">ASP.NET Core ignoruje [Ogólne kontrolery](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="ac534-152">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="ac534-153">Kontroler generyczny ma parametr typu (na przykład `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="ac534-153">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="ac534-154">Poniższy przykład dodaje wystąpienia kontrolera ogólnego dla określonej listy typów:</span><span class="sxs-lookup"><span data-stu-id="ac534-154">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="ac534-155">Typy są zdefiniowane w `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="ac534-155">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="ac534-156">Dostawca funkcji został dodany w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="ac534-156">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="ac534-157">Nazwy kontrolerów ogólnych używane do routingu mają postać *GenericController ' 1 [widget]* , a nie *widżetu*.</span><span class="sxs-lookup"><span data-stu-id="ac534-157">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="ac534-158">Następujący atrybut modyfikuje nazwę odpowiadającą typowi ogólnemu używanemu przez kontroler:</span><span class="sxs-lookup"><span data-stu-id="ac534-158">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="ac534-159">`GenericController` Klasa:</span><span class="sxs-lookup"><span data-stu-id="ac534-159">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="ac534-160">Na przykład żądanie adresu URL `https://localhost:5001/Sprocket` wyników w następującej odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="ac534-160">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="ac534-161">Wyświetlanie dostępnych funkcji</span><span class="sxs-lookup"><span data-stu-id="ac534-161">Display available features</span></span>

<span data-ttu-id="ac534-162">Funkcje dostępne dla aplikacji można wyliczyć, żądając `ApplicationPartManager` od [iniekcji zależności](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="ac534-162">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="ac534-163">[Przykład pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) używa powyższego kodu, aby wyświetlić funkcje aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ac534-163">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

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
