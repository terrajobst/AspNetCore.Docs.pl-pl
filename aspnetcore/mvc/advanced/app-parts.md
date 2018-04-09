---
title: Części aplikacji platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać części aplikacji, które są abstrakcje nad zasobami aplikacji, do odnajdowania lub uniknąć obciążania funkcji z zestawu.
manager: wpickett
ms.author: riande
ms.date: 01/04/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 31ab125462da032d30ef25b45cf81a0d18212bc1
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2018
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="54c43-103">Części aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54c43-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="54c43-104">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="54c43-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="54c43-105">*Część aplikacji* jest abstrakcję przy użyciu zasobów aplikacji, z którego MVC funkcjami, takimi jak kontrolery, składniki widoku lub pomocników tagów może zostać odnalezione.</span><span class="sxs-lookup"><span data-stu-id="54c43-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="54c43-106">Przykładem części aplikacji jest AssemblyPart, który hermetyzuje odwołania do zestawu i ujawnia typy i odwołuje się do kompilacji.</span><span class="sxs-lookup"><span data-stu-id="54c43-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="54c43-107">*Funkcja dostawców* pracować z części aplikacji do wypełniania funkcji aplikacji ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="54c43-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="54c43-108">Przypadek użycia głównego dla części aplikacji jest umożliwienie użytkownikom skonfiguruj aplikację, aby odnaleźć (lub Unikaj ładowania) z zestawu funkcji MVC.</span><span class="sxs-lookup"><span data-stu-id="54c43-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="54c43-109">Wprowadzenie do części aplikacji</span><span class="sxs-lookup"><span data-stu-id="54c43-109">Introducing Application Parts</span></span>

<span data-ttu-id="54c43-110">Aplikacji MVC załadować ich funkcje z [części aplikacji](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="54c43-110">MVC apps load their features from [application parts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="54c43-111">W szczególności [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) klasa reprezentuje część aplikacji, która nie jest obsługiwana przez zestaw.</span><span class="sxs-lookup"><span data-stu-id="54c43-111">In particular, the [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="54c43-112">Aby odnaleźć i załadować MVC funkcje, takie jak kontrolerów, wyświetlania składników pomocników tagów i źródeł kompilacji razor, można użyć tych klas.</span><span class="sxs-lookup"><span data-stu-id="54c43-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="54c43-113">[ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) jest odpowiedzialny za śledzenia części aplikacji i dostawców funkcji dostępnych w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="54c43-113">The [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="54c43-114">Użytkownik może interakcyjnie przeprowadzić `ApplicationPartManager` w `Startup` podczas konfigurowania MVC:</span><span class="sxs-lookup"><span data-stu-id="54c43-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

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

<span data-ttu-id="54c43-115">Domyślnie MVC przeszukać drzewo zależności i znaleźć kontrolery (nawet w innych zestawów).</span><span class="sxs-lookup"><span data-stu-id="54c43-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="54c43-116">Aby załadować dowolnego zestawu (na przykład z wtyczkę, która nie jest przywoływany w czasie kompilacji), można użyć część aplikacji.</span><span class="sxs-lookup"><span data-stu-id="54c43-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="54c43-117">Można użyć części aplikacji do *uniknąć* wyszukiwanie kontrolerów z określonego zestawu lub lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="54c43-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="54c43-118">Można kontrolować, które części (lub zestawy) są dostępne dla aplikacji, modyfikując `ApplicationParts` kolekcji `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="54c43-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="54c43-119">Kolejność pozycji w `ApplicationParts` kolekcji nie jest ważna.</span><span class="sxs-lookup"><span data-stu-id="54c43-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="54c43-120">Ważne jest, aby skonfigurować w pełni `ApplicationPartManager` przed użyciem jej do skonfigurowania usług w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="54c43-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="54c43-121">Na przykład pełni należy skonfigurować `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="54c43-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="54c43-122">Można to zrobić, będzie oznaczać, że kontrolerów w części aplikacji dodane po wywołanie metody nie będzie mieć wpływ (nie pobrać zarejestrowany jako usługi) może spowodować niepoprawne bevavior aplikacji.</span><span class="sxs-lookup"><span data-stu-id="54c43-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="54c43-123">Jeśli masz zestaw, który zawiera kontrolery nie chcesz do użycia, usunąć go z `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="54c43-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="54c43-124">Oprócz zestawu projektu i jego zestawów zależnych `ApplicationPartManager` będzie zawierać części dla `Microsoft.AspNetCore.Mvc.TagHelpers` i `Microsoft.AspNetCore.Mvc.Razor` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="54c43-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="54c43-125">Dostawcy funkcji aplikacji</span><span class="sxs-lookup"><span data-stu-id="54c43-125">Application Feature Providers</span></span>

<span data-ttu-id="54c43-126">Dostawcy funkcji aplikacji Sprawdź części aplikacji i udostępnia funkcje dla tych elementów.</span><span class="sxs-lookup"><span data-stu-id="54c43-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="54c43-127">Brak dostawców wbudowanych funkcji MVC następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="54c43-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="54c43-128">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="54c43-128">Controllers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="54c43-129">Odwołanie do metadanych</span><span class="sxs-lookup"><span data-stu-id="54c43-129">Metadata Reference</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="54c43-130">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="54c43-130">Tag Helpers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="54c43-131">Składniki w widoku</span><span class="sxs-lookup"><span data-stu-id="54c43-131">View Components</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="54c43-132">Dziedzicz dostawców funkcji `IApplicationFeatureProvider<T>`, gdzie `T` jest typem funkcji.</span><span class="sxs-lookup"><span data-stu-id="54c43-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="54c43-133">Można zaimplementować własnych funkcji dostawców dla dowolnego typu funkcji MVC wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="54c43-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="54c43-134">Kolejność dostawców funkcji w `ApplicationPartManager.FeatureProviders` kolekcji może być ważne, ponieważ nowsze dostawców można reagować na akcje wykonywane przez poprzednie dostawców.</span><span class="sxs-lookup"><span data-stu-id="54c43-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="54c43-135">Przykład: Funkcja ogólnego kontrolera</span><span class="sxs-lookup"><span data-stu-id="54c43-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="54c43-136">Domyślnie program ASP.NET Core MVC ignoruje ogólnego kontrolerów (na przykład `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="54c43-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="54c43-137">W przykładzie użyto dostawcy funkcji kontrolera, który jest uruchamiany po domyślnego dostawcy i dodaje wystąpienia kontrolera ogólne dla określonej listy typów (zdefiniowany w `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="54c43-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="54c43-138">Typy jednostek:</span><span class="sxs-lookup"><span data-stu-id="54c43-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="54c43-139">Dostawca funkcji został dodany w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="54c43-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="54c43-140">Domyślnie nazwy kontrolera ogólnego używane do przesyłania będzie mieć postać *GenericController'1 [Widget]* zamiast *elementu Widget*.</span><span class="sxs-lookup"><span data-stu-id="54c43-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="54c43-141">Następującego atrybutu służy do modyfikowania nazwy odpowiadającej typu ogólnego używane przez kontrolera:</span><span class="sxs-lookup"><span data-stu-id="54c43-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="54c43-142">`GenericController` Klasy:</span><span class="sxs-lookup"><span data-stu-id="54c43-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="54c43-143">Wynik, gdy wymagane jest pasującej trasy:</span><span class="sxs-lookup"><span data-stu-id="54c43-143">The result, when a matching route is requested:</span></span>

![Przykładowe dane wyjściowe z przykładowej aplikacji odczytuje powitania od kontrolera Sproket ogólnego.](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="54c43-145">Przykład: Wyświetlanie dostępnych funkcji</span><span class="sxs-lookup"><span data-stu-id="54c43-145">Sample: Display available features</span></span>

<span data-ttu-id="54c43-146">Można wykonać iterację wypełnione funkcje dostępne do aplikacji przez zażądanie `ApplicationPartManager` za pośrednictwem [iniekcji zależności](../../fundamentals/dependency-injection.md) i używanie go można wypełnić wystąpień odpowiednie funkcje:</span><span class="sxs-lookup"><span data-stu-id="54c43-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="54c43-147">Przykład danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="54c43-147">Example output:</span></span>

![Przykładowe dane wyjściowe z przykładowej aplikacji](app-parts/_static/available-features.png)
