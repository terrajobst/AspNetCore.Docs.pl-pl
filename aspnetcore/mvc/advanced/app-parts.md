---
title: Części aplikacji w ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać części aplikacji, które są abstrakcją za pośrednictwem zasobów aplikacji, aby odnaleźć lub uniknąć ładowania funkcji z zestawu.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4900ccf5589500db076f8cecd9da198c6a7ceea4
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886465"
---
<!-- DO NOT MAKE CHANGES BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12376 Merges -->

# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="b6c04-103">Części aplikacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6c04-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="b6c04-104">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b6c04-104">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b6c04-105">*Część aplikacji* to Abstrakcja zasobów aplikacji, z której mogą zostać odnalezione funkcje MVC, takie jak kontrolery, składniki widoku lub pomocniki tagów.</span><span class="sxs-lookup"><span data-stu-id="b6c04-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="b6c04-106">Przykładem części aplikacji jest AssemblyPart, który hermetyzuje odwołanie do zestawu i ujawnia typy i odwołania do kompilacji.</span><span class="sxs-lookup"><span data-stu-id="b6c04-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="b6c04-107">*Dostawcy funkcji* pracują z częściami aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="b6c04-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="b6c04-108">Głównym przypadkiem użycia części aplikacji jest umożliwienie konfigurowania aplikacji w celu odnajdywania (lub unikania ładowania) funkcji MVC z zestawu.</span><span class="sxs-lookup"><span data-stu-id="b6c04-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="b6c04-109">Wprowadzenie do części aplikacji</span><span class="sxs-lookup"><span data-stu-id="b6c04-109">Introducing Application Parts</span></span>

<span data-ttu-id="b6c04-110">Aplikacje MVC ładują swoje funkcje z [części aplikacji](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="b6c04-110">MVC apps load their features from [application parts](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="b6c04-111">W szczególności Klasa [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart) reprezentuje część aplikacji, która jest obsługiwana przez zestaw.</span><span class="sxs-lookup"><span data-stu-id="b6c04-111">In particular, the [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="b6c04-112">Tych klas można używać do odnajdywania i ładowania funkcji MVC, takich jak kontrolery, składniki widoku, pomocnicy tagów i źródła kompilacji Razor.</span><span class="sxs-lookup"><span data-stu-id="b6c04-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="b6c04-113">[ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) jest odpowiedzialny za śledzenie składników aplikacji i dostawców funkcji dostępnych dla aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="b6c04-113">The [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="b6c04-114">Podczas konfigurowania MVC można korzystać `ApplicationPartManager` z `Startup` programu w programie:</span><span class="sxs-lookup"><span data-stu-id="b6c04-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

<span data-ttu-id="b6c04-115">Domyślnie MVC przeszuka drzewo zależności i znajdzie kontrolery (nawet w innych zestawach).</span><span class="sxs-lookup"><span data-stu-id="b6c04-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="b6c04-116">Do załadowania dowolnego zestawu (na przykład z wtyczki, do której nie odwołuje się w czasie kompilacji), można użyć części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6c04-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="b6c04-117">Możesz użyć części aplikacji, aby *uniknąć* wyszukiwania kontrolerów w określonym zestawie lub lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b6c04-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="b6c04-118">Można kontrolować, które części (lub zestawy) są dostępne dla aplikacji, modyfikując `ApplicationParts` kolekcję. `ApplicationPartManager`</span><span class="sxs-lookup"><span data-stu-id="b6c04-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="b6c04-119">Kolejność wpisów w `ApplicationParts` kolekcji nie jest ważna.</span><span class="sxs-lookup"><span data-stu-id="b6c04-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="b6c04-120">Ważne jest, `ApplicationPartManager` aby w pełni skonfigurować usługę przed jej użyciem do konfigurowania usług w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="b6c04-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="b6c04-121">Na przykład należy w pełni skonfigurować `ApplicationPartManager` przed wywołaniem. `AddControllersAsServices`</span><span class="sxs-lookup"><span data-stu-id="b6c04-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="b6c04-122">W przeciwnym razie oznacza to, że nie będzie to miało wpływu na kontrolery w składnikach aplikacji dodanych po wywołaniu tej metody (nie zostaną one zarejestrowane jako usługi), co może spowodować nieprawidłowe zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6c04-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect behavior of your application.</span></span>

<span data-ttu-id="b6c04-123">Jeśli masz zestaw zawierający kontrolery, których nie chcesz używać, usuń go z `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="b6c04-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="b6c04-124">Oprócz zestawu projektu i jego zestawów zależnych, `ApplicationPartManager` program będzie zawierać części dla `Microsoft.AspNetCore.Mvc.TagHelpers` i `Microsoft.AspNetCore.Mvc.Razor` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="b6c04-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="b6c04-125">Dostawcy funkcji aplikacji</span><span class="sxs-lookup"><span data-stu-id="b6c04-125">Application Feature Providers</span></span>

<span data-ttu-id="b6c04-126">Dostawcy funkcji aplikacji badają części aplikacji i udostępniają funkcje dla tych części.</span><span class="sxs-lookup"><span data-stu-id="b6c04-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="b6c04-127">Istnieją Wbudowani dostawcy funkcji dla następujących funkcji MVC:</span><span class="sxs-lookup"><span data-stu-id="b6c04-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="b6c04-128">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="b6c04-128">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="b6c04-129">Odwołanie do metadanych</span><span class="sxs-lookup"><span data-stu-id="b6c04-129">Metadata Reference</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="b6c04-130">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="b6c04-130">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="b6c04-131">Wyświetl składniki</span><span class="sxs-lookup"><span data-stu-id="b6c04-131">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="b6c04-132">Dostawcy funkcji dziedziczą `IApplicationFeatureProvider<T>`z, `T` gdzie jest typem funkcji.</span><span class="sxs-lookup"><span data-stu-id="b6c04-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="b6c04-133">Możesz zaimplementować własnych dostawców funkcji dla dowolnego z wymienionych powyżej typów funkcji MVC.</span><span class="sxs-lookup"><span data-stu-id="b6c04-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="b6c04-134">Kolejność dostawców funkcji w `ApplicationPartManager.FeatureProviders` kolekcji może być ważna, ponieważ później dostawcy mogą reagować na działania podejmowane przez wcześniejszych dostawców.</span><span class="sxs-lookup"><span data-stu-id="b6c04-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="b6c04-135">Northwind Funkcja kontrolera ogólnego</span><span class="sxs-lookup"><span data-stu-id="b6c04-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="b6c04-136">Domyślnie ASP.NET Core MVC ignoruje ogólne kontrolery (na przykład `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="b6c04-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="b6c04-137">Ten przykład korzysta z dostawcy funkcji kontrolera, który jest uruchamiany po domyślnym dostawcy i dodaje wystąpienia kontrolera ogólnego dla określonej listy typów (zdefiniowane w `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="b6c04-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="b6c04-138">Typy jednostek:</span><span class="sxs-lookup"><span data-stu-id="b6c04-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="b6c04-139">Dostawca funkcji został dodany w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b6c04-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="b6c04-140">Domyślnie nazwy kontrolerów ogólnych używane do routingu byłyby postać *GenericController ' 1 [widget]* zamiast *widgetu*.</span><span class="sxs-lookup"><span data-stu-id="b6c04-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="b6c04-141">Następujący atrybut służy do modyfikowania nazwy, aby odpowiadała typowi ogólnemu używanemu przez kontroler:</span><span class="sxs-lookup"><span data-stu-id="b6c04-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="b6c04-142">`GenericController` Klasa:</span><span class="sxs-lookup"><span data-stu-id="b6c04-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="b6c04-143">Wynik, gdy żąda się zgodnej trasy:</span><span class="sxs-lookup"><span data-stu-id="b6c04-143">The result, when a matching route is requested:</span></span>

![Przykładowe dane wyjściowe z przykładowej aplikacji odczytuje "Hello z ogólnego kontrolera Sprocket".](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="b6c04-145">Northwind Wyświetlanie dostępnych funkcji</span><span class="sxs-lookup"><span data-stu-id="b6c04-145">Sample: Display available features</span></span>

<span data-ttu-id="b6c04-146">Można wykonać iterację wypełnionych funkcji dostępnych dla aplikacji, żądając `ApplicationPartManager` od iniekcji [zależności](../../fundamentals/dependency-injection.md) i używając go do wypełniania wystąpień odpowiednich funkcji:</span><span class="sxs-lookup"><span data-stu-id="b6c04-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="b6c04-147">Przykładowe dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="b6c04-147">Example output:</span></span>

![Przykładowe dane wyjściowe z przykładowej aplikacji](app-parts/_static/available-features.png)
