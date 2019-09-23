---
title: Części aplikacji w ASP.NET Core
author: rick-anderson
description: Udostępnianie kontrolerów, przeglądanie, Razor Pages i nie tylko za pomocą części aplikacji w ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: ad0372f25377115e6fc7c8ea42db75de56b3e6d2
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187013"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="9be27-103">Udostępnianie kontrolerów, widoków, Razor Pages i innych elementów aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9be27-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>
=======

<!-- DO NOT MAKE CHANGES BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12376 Merges -->

<span data-ttu-id="9be27-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9be27-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9be27-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9be27-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9be27-106">*Część aplikacji* to Abstrakcja zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9be27-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="9be27-107">Części aplikacji umożliwiają ASP.NET Core odnajdywania kontrolerów, wyświetlania składników, pomocników tagów, Razor Pages, źródeł kompilacji Razor i nie tylko.</span><span class="sxs-lookup"><span data-stu-id="9be27-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="9be27-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) jest częścią aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9be27-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="9be27-109">`AssemblyPart`hermetyzuje odwołanie do zestawu i udostępnia typy i odwołania do kompilacji.</span><span class="sxs-lookup"><span data-stu-id="9be27-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="9be27-110">*Dostawcy funkcji* pracują z częściami aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9be27-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="9be27-111">Głównym przypadkiem użycia części aplikacji jest skonfigurowanie aplikacji do odnajdywania (lub unikania ładowania) ASP.NET Core funkcji z zestawu.</span><span class="sxs-lookup"><span data-stu-id="9be27-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="9be27-112">Na przykład może być konieczne udostępnienie typowych funkcji między wieloma aplikacjami.</span><span class="sxs-lookup"><span data-stu-id="9be27-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="9be27-113">Korzystając z części aplikacji, można udostępnić zestaw (DLL) zawierający kontrolery, widoki, Razor Pages, źródła kompilacji Razor, pomocników tagów i wiele innych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9be27-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="9be27-114">Udostępnianie zestawu jest preferowane do duplikowania kodu w wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="9be27-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="9be27-115">ASP.NET Core aplikacje ładują funkcje <xref:System.Web.WebPages.ApplicationPart>z programu.</span><span class="sxs-lookup"><span data-stu-id="9be27-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="9be27-116"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> Klasa reprezentuje część aplikacji, która jest obsługiwana przez zestaw.</span><span class="sxs-lookup"><span data-stu-id="9be27-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="9be27-117">Załaduj funkcje ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9be27-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="9be27-118">Użyj klas `AssemblyPart` i, aby odnajdywać i ładować funkcje ASP.NET Core (kontrolery, składniki widoku itp.). `ApplicationPart`</span><span class="sxs-lookup"><span data-stu-id="9be27-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="9be27-119"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> Śledzi dostępne części aplikacji i dostawców funkcji.</span><span class="sxs-lookup"><span data-stu-id="9be27-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="9be27-120">`ApplicationPartManager`jest skonfigurowany w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9be27-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="9be27-121">Poniższy kod stanowi alternatywny sposób konfigurowania `ApplicationPartManager` użycia: `AssemblyPart`</span><span class="sxs-lookup"><span data-stu-id="9be27-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="9be27-122">Powyższe dwa przykłady kodu ładują `SharedController` z zestawu.</span><span class="sxs-lookup"><span data-stu-id="9be27-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="9be27-123">Nie `SharedController` znajduje się w projekcie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9be27-123">The `SharedController` is not in the applications project.</span></span> <span data-ttu-id="9be27-124">Zobacz Pobieranie przykładowego [rozwiązania WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="9be27-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="9be27-125">Dołącz widoki</span><span class="sxs-lookup"><span data-stu-id="9be27-125">Include views</span></span>

<span data-ttu-id="9be27-126">Aby uwzględnić widoki w zestawie:</span><span class="sxs-lookup"><span data-stu-id="9be27-126">To include views in the assembly:</span></span>

* <span data-ttu-id="9be27-127">Dodaj następujący znacznik do pliku projektu udostępnionego:</span><span class="sxs-lookup"><span data-stu-id="9be27-127">Add the following markup to the shared project file:</span></span>

  ```csproj
    <ItemGroup>
      <EmbeddedResource Include = "Views\**\*.cshtml" />
    </ ItemGroup >
  ```

* <span data-ttu-id="9be27-128"><xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> Dodaj<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>do:</span><span class="sxs-lookup"><span data-stu-id="9be27-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="9be27-129">Zapobiegaj ładowaniu zasobów</span><span class="sxs-lookup"><span data-stu-id="9be27-129">Prevent loading resources</span></span>

<span data-ttu-id="9be27-130">Części aplikacji mogą służyć do *uniknięcia* ładowania zasobów w określonym zestawie lub lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="9be27-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="9be27-131">Dodaj lub Usuń elementy członkowskie <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> kolekcji, aby ukryć lub udostępnić dostępne zasoby.</span><span class="sxs-lookup"><span data-stu-id="9be27-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="9be27-132">Kolejność wpisów w `ApplicationParts` kolekcji nie jest ważna.</span><span class="sxs-lookup"><span data-stu-id="9be27-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="9be27-133">Skonfiguruj program `ApplicationPartManager` przed użyciem go do konfigurowania usług w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="9be27-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="9be27-134">Na przykład skonfiguruj `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="9be27-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="9be27-135">Wywołaj `Remove`kolekcję,abyusunąć zasób.`ApplicationParts`</span><span class="sxs-lookup"><span data-stu-id="9be27-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="9be27-136">Następujący kod używa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> do usunięcia `MyDependentLibrary` z aplikacji:[!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="9be27-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="9be27-137">`ApplicationPartManager` Zawiera części dla:</span><span class="sxs-lookup"><span data-stu-id="9be27-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="9be27-138">Zestaw aplikacji i zestawy zależne.</span><span class="sxs-lookup"><span data-stu-id="9be27-138">The apps assembly and dependent assemblies.</span></span>
* `Microsoft.AspNetCore.Mvc.TagHelpers`
* <span data-ttu-id="9be27-139">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="9be27-139">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="9be27-140">Dostawcy funkcji aplikacji</span><span class="sxs-lookup"><span data-stu-id="9be27-140">Application feature providers</span></span>

<span data-ttu-id="9be27-141">Dostawcy funkcji aplikacji badają części aplikacji i udostępniają funkcje dla tych części.</span><span class="sxs-lookup"><span data-stu-id="9be27-141">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="9be27-142">Istnieją Wbudowani dostawcy funkcji dla następujących funkcji ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="9be27-142">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="9be27-143">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="9be27-143">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="9be27-144">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="9be27-144">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="9be27-145">Wyświetl składniki</span><span class="sxs-lookup"><span data-stu-id="9be27-145">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="9be27-146">Dostawcy funkcji dziedziczą <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>z, `T` gdzie jest typem funkcji.</span><span class="sxs-lookup"><span data-stu-id="9be27-146">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="9be27-147">Dostawców funkcji można zaimplementować dla dowolnego z wcześniej wymienionych typów funkcji.</span><span class="sxs-lookup"><span data-stu-id="9be27-147">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="9be27-148">Kolejność dostawców funkcji w programie `ApplicationPartManager.FeatureProviders` może mieć wpływ na zachowanie.</span><span class="sxs-lookup"><span data-stu-id="9be27-148">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="9be27-149">Później dodani dostawcy mogą reagować na akcje podejmowane przez wcześniej dodanych dostawców.</span><span class="sxs-lookup"><span data-stu-id="9be27-149">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="9be27-150">Funkcja kontrolera ogólnego</span><span class="sxs-lookup"><span data-stu-id="9be27-150">Generic controller feature</span></span>

<span data-ttu-id="9be27-151">ASP.NET Core ignoruje [Ogólne kontrolery](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="9be27-151">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="9be27-152">Kontroler generyczny ma parametr typu (na przykład `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="9be27-152">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="9be27-153">Poniższy przykład dodaje wystąpienia kontrolera ogólnego dla określonej listy typów.</span><span class="sxs-lookup"><span data-stu-id="9be27-153">The following sample adds generic controller instances for a specified list of types.</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="9be27-154">Typy są zdefiniowane w `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="9be27-154">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="9be27-155">Dostawca funkcji został dodany w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="9be27-155">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="9be27-156">Nazwy kontrolerów ogólnych używane do routingu mają postać *GenericController ' 1 [widget]* , a nie *widżetu*.</span><span class="sxs-lookup"><span data-stu-id="9be27-156">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="9be27-157">Następujący atrybut modyfikuje nazwę odpowiadającą typowi ogólnemu używanemu przez kontroler:</span><span class="sxs-lookup"><span data-stu-id="9be27-157">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="9be27-158">`GenericController` Klasa:</span><span class="sxs-lookup"><span data-stu-id="9be27-158">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

### <a name="display-available-features"></a><span data-ttu-id="9be27-159">Wyświetlanie dostępnych funkcji</span><span class="sxs-lookup"><span data-stu-id="9be27-159">Display available features</span></span>

<span data-ttu-id="9be27-160">Funkcje dostępne dla aplikacji można wyliczyć przez zażądanie `ApplicationPartManager` [iniekcji zależności](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="9be27-160">The features available to an app can be enumerated by by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="9be27-161">[Przykład pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) używa poprzedniego kodu do wyświetlania funkcji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9be27-161">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features.</span></span>