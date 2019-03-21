---
title: Części aplikacji w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak za pomocą części aplikacji, które abstrakcje nad zasobami aplikacji, wykrywanie i uniknąć obciążania funkcji z zestawu.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 67bd40adef4cdb0bd781f70114d3954cd9a8ed09
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320098"
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="0983d-103">Części aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0983d-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="0983d-104">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0983d-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0983d-105">*Aplikacji część* jest klasą abstrakcyjną nad zasobami aplikacji, z którego MVC funkcjami, jak kontrolerów, składniki widoków lub może zostać odnalezionych pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="0983d-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="0983d-106">Jednym z przykładów aplikacji, część jest AssemblyPart, który hermetyzuje odwołania do zestawu i ujawnia typy i odwołania do kompilacji.</span><span class="sxs-lookup"><span data-stu-id="0983d-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="0983d-107">*Funkcja dostawców* działają z części aplikacji, aby wypełnić funkcje aplikacji ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="0983d-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="0983d-108">Głównie w przypadku części aplikacji jest umożliwienie skonfiguruj aplikację, aby odnaleźć (lub uniknąć ładowanie) funkcjami MVC z zestawu.</span><span class="sxs-lookup"><span data-stu-id="0983d-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="0983d-109">Wprowadzenie do części aplikacji</span><span class="sxs-lookup"><span data-stu-id="0983d-109">Introducing Application Parts</span></span>

<span data-ttu-id="0983d-110">Aplikacje MVC ładować swoje funkcje [części aplikacji](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="0983d-110">MVC apps load their features from [application parts](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="0983d-111">W szczególności [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) klasa reprezentuje część aplikacji, która jest wspierana przez zespół.</span><span class="sxs-lookup"><span data-stu-id="0983d-111">In particular, the [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="0983d-112">W ramach tych zajęć umożliwia wykrycie i załadowanie funkcji MVC, takich jak kontrolery, składniki widoków, pomocników tagów i źródła kompilacji razor.</span><span class="sxs-lookup"><span data-stu-id="0983d-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="0983d-113">[ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) jest odpowiedzialny za śledzenie części aplikacji i dostawców funkcji dostępnych w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="0983d-113">The [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="0983d-114">Możesz wchodzić w interakcje z `ApplicationPartManager` w `Startup` po skonfigurowaniu MVC:</span><span class="sxs-lookup"><span data-stu-id="0983d-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

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

<span data-ttu-id="0983d-115">Domyślnie MVC przeszukać drzewo zależności i znaleźć kontrolery (nawet w przypadku innych zestawów).</span><span class="sxs-lookup"><span data-stu-id="0983d-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="0983d-116">Aby załadować dowolny zestaw (na przykład z dodatek, który nie jest przywoływany w czasie kompilacji), można użyć części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0983d-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="0983d-117">Możesz użyć aplikacji składa się z części *uniknąć* szukasz kontrolerów z określonego zestawu lub lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="0983d-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="0983d-118">Można kontrolować, które części (lub zespoły) są dostępne dla aplikacji, modyfikując `ApplicationParts` zbiór `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="0983d-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="0983d-119">Kolejność wpisy w `ApplicationParts` kolekcji nie są ważne.</span><span class="sxs-lookup"><span data-stu-id="0983d-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="0983d-120">Jest ważne w pełni skonfigurować `ApplicationPartManager` przed jego użyciem, aby skonfigurować usługi w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="0983d-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="0983d-121">Na przykład w pełni należy skonfigurować `ApplicationPartManager` przed wywołaniem `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="0983d-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="0983d-122">Kończy się niepowodzeniem to zrobić, oznacza, że kontrolerów w części aplikacji dodane po wywołanie metody nie ulegnie zmianie (nie będzie Zarejestruj się jako usługi) może spowodować niepoprawne działanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0983d-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect behavior of your application.</span></span>

<span data-ttu-id="0983d-123">Jeśli masz zestaw, który zawiera kontrolery nie chcesz być używane, usuń go z `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="0983d-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

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

<span data-ttu-id="0983d-124">Oprócz zestawu projektu i jego zestawów zależnych `ApplicationPartManager` będzie zawierać części do `Microsoft.AspNetCore.Mvc.TagHelpers` i `Microsoft.AspNetCore.Mvc.Razor` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="0983d-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="0983d-125">Dostawcy funkcji aplikacji</span><span class="sxs-lookup"><span data-stu-id="0983d-125">Application Feature Providers</span></span>

<span data-ttu-id="0983d-126">Dostawcy funkcji aplikacji Sprawdź części aplikacji i udostępnia funkcje w tych częściach.</span><span class="sxs-lookup"><span data-stu-id="0983d-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="0983d-127">Dostępne są następujące funkcje MVC dostawców wbudowanej funkcji:</span><span class="sxs-lookup"><span data-stu-id="0983d-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="0983d-128">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="0983d-128">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="0983d-129">Odwołanie do metadanych</span><span class="sxs-lookup"><span data-stu-id="0983d-129">Metadata Reference</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="0983d-130">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="0983d-130">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="0983d-131">Składniki widoków</span><span class="sxs-lookup"><span data-stu-id="0983d-131">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="0983d-132">Dziedzicz dostawców funkcji `IApplicationFeatureProvider<T>`, gdzie `T` jest typem funkcji.</span><span class="sxs-lookup"><span data-stu-id="0983d-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="0983d-133">Można zaimplementować własną funkcji, którą dostawców dla dowolnego typu funkcji MVC wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="0983d-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="0983d-134">Kolejność dostawców funkcji w `ApplicationPartManager.FeatureProviders` kolekcji może być istotne, od dostawców nowszego pozwala reagować na akcje wykonywane przez dostawców poprzedniego.</span><span class="sxs-lookup"><span data-stu-id="0983d-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="0983d-135">Przykład: Funkcja ogólna kontrolera</span><span class="sxs-lookup"><span data-stu-id="0983d-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="0983d-136">Domyślnie program ASP.NET Core MVC ignoruje ogólnego kontrolerów (na przykład `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="0983d-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="0983d-137">W tym przykładzie użyto dostawcy funkcji kontrolera, który jest uruchamiany po domyślnego dostawcę i dodaje wystąpień ogólnego kontrolera dla określonej listy typów (zdefiniowane w `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="0983d-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="0983d-138">Typy jednostek:</span><span class="sxs-lookup"><span data-stu-id="0983d-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="0983d-139">Dostawca funkcji zostanie dodany do `Startup`:</span><span class="sxs-lookup"><span data-stu-id="0983d-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="0983d-140">Domyślnie nazwy rodzajowe kontrolerów, używany do routingu będzie mieć postać *GenericController'1 [widżet]* zamiast *widżet*.</span><span class="sxs-lookup"><span data-stu-id="0983d-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="0983d-141">Następujący atrybut jest używane do modyfikowania nazwy odpowiadają typu ogólnego, używane przez kontroler:</span><span class="sxs-lookup"><span data-stu-id="0983d-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="0983d-142">`GenericController` Klasy:</span><span class="sxs-lookup"><span data-stu-id="0983d-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="0983d-143">Wynik, zleconą pasującej trasy:</span><span class="sxs-lookup"><span data-stu-id="0983d-143">The result, when a matching route is requested:</span></span>

![Przykładowe dane wyjściowe z przykładowej aplikacji odczytuje pozdrowienia z kontrolera zębnik ogólnego.](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="0983d-145">Przykład: Wyświetlanie dostępnych funkcji</span><span class="sxs-lookup"><span data-stu-id="0983d-145">Sample: Display available features</span></span>

<span data-ttu-id="0983d-146">Można wykonać iterację wypełnione funkcji dostępnych do swojej aplikacji, wysyłając żądanie `ApplicationPartManager` za pośrednictwem [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) i używanie ich do wystąpienia odpowiednie funkcje:</span><span class="sxs-lookup"><span data-stu-id="0983d-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="0983d-147">Przykładowe dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="0983d-147">Example output:</span></span>

![Przykładowe dane wyjściowe z przykładowej aplikacji](app-parts/_static/available-features.png)
