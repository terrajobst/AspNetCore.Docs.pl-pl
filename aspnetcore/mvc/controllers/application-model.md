---
title: Praca z modelem aplikacji w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak odczytywanie i przetwarzanie modelu aplikacji, aby zmodyfikować zachowanie elementów MVC platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/application-model
ms.openlocfilehash: 6b0591a877c0d82e0ee6ab002eb6a6650753677b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903178"
---
# <a name="work-with-the-application-model-in-aspnet-core"></a><span data-ttu-id="568a5-103">Praca z modelem aplikacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="568a5-103">Work with the application model in ASP.NET Core</span></span>

<span data-ttu-id="568a5-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="568a5-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="568a5-105">Definiuje ASP.NET Core MVC *model aplikacji* reprezentująca składniki aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="568a5-105">ASP.NET Core MVC defines an *application model* representing the components of an MVC app.</span></span> <span data-ttu-id="568a5-106">Może odczytywać i modyfikować ten model, aby zmodyfikować zachowanie elementów MVC.</span><span class="sxs-lookup"><span data-stu-id="568a5-106">You can read and manipulate this model to modify how MVC elements behave.</span></span> <span data-ttu-id="568a5-107">Domyślnie program MVC zgodna z konwencjami niektórych klas, które są uważane za kontrolerów, metod na tych klas są akcje i zachowaniem parametrów i routing.</span><span class="sxs-lookup"><span data-stu-id="568a5-107">By default, MVC follows certain conventions to determine which classes are considered to be controllers, which methods on those classes are actions, and how parameters and routing behave.</span></span> <span data-ttu-id="568a5-108">Można dostosować to zachowanie do potrzeb Twojej aplikacji przez utworzenie własnych Konwencji odpowiadającym i stosując je globalnie lub jako atrybuty.</span><span class="sxs-lookup"><span data-stu-id="568a5-108">You can customize this behavior to suit your app's needs by creating your own conventions and applying them globally or as attributes.</span></span>

## <a name="models-and-providers"></a><span data-ttu-id="568a5-109">Modele i dostawców</span><span class="sxs-lookup"><span data-stu-id="568a5-109">Models and Providers</span></span>

<span data-ttu-id="568a5-110">Model aplikacji ASP.NET Core MVC obejmują zarówno abstrakcyjne interfejsów, jak i klasy konkretnej implementacji, które opisują aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="568a5-110">The ASP.NET Core MVC application model include both abstract interfaces and concrete implementation classes that describe an MVC application.</span></span> <span data-ttu-id="568a5-111">Ten model jest wynikiem odnajdywanie kontrolerów, akcji, parametrów akcji, trasy i filtrów w zależności od domyślnych Konwencji aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="568a5-111">This model is the result of MVC discovering the app's controllers, actions, action parameters, routes, and filters according to default conventions.</span></span> <span data-ttu-id="568a5-112">Poprzez współdziałanie z modelem aplikacji, można zmodyfikować aplikację z różnych Konwencji z domyślnym zachowaniem platformy MVC.</span><span class="sxs-lookup"><span data-stu-id="568a5-112">By working with the application model, you can modify your app to follow different conventions from the default MVC behavior.</span></span> <span data-ttu-id="568a5-113">Parametry nazwy, trasy i filtry są wszystkie używane jako dane konfiguracji dla akcji i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="568a5-113">The parameters, names, routes, and filters are all used as configuration data for actions and controllers.</span></span>

<span data-ttu-id="568a5-114">Model aplikacji platformy ASP.NET Core MVC ma następującą strukturę:</span><span class="sxs-lookup"><span data-stu-id="568a5-114">The ASP.NET Core MVC Application Model has the following structure:</span></span>

* <span data-ttu-id="568a5-115">ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="568a5-115">ApplicationModel</span></span>
  * <span data-ttu-id="568a5-116">Kontrolery (ControllerModel)</span><span class="sxs-lookup"><span data-stu-id="568a5-116">Controllers (ControllerModel)</span></span>
    * <span data-ttu-id="568a5-117">Akcje (ActionModel)</span><span class="sxs-lookup"><span data-stu-id="568a5-117">Actions (ActionModel)</span></span>
      * <span data-ttu-id="568a5-118">Parametry (ParameterModel)</span><span class="sxs-lookup"><span data-stu-id="568a5-118">Parameters (ParameterModel)</span></span>

<span data-ttu-id="568a5-119">Każdy poziom modelu ma dostęp do wspólnego `Properties` zbierania i niższych poziomach dostępu i zastąpić wartości właściwości ustawione przez wyższy poziom w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="568a5-119">Each level of the model has access to a common `Properties` collection, and lower levels can access and overwrite property values set by higher levels in the hierarchy.</span></span> <span data-ttu-id="568a5-120">Właściwości zostaną utrwalone w `ActionDescriptor.Properties` utworzenia akcji.</span><span class="sxs-lookup"><span data-stu-id="568a5-120">The properties are persisted to the `ActionDescriptor.Properties` when the actions are created.</span></span> <span data-ttu-id="568a5-121">Następnie podczas obsługi żądania żadnych właściwości Konwencji dodawany lub modyfikowany jest możliwy za pośrednictwem `ActionContext.ActionDescriptor.Properties`.</span><span class="sxs-lookup"><span data-stu-id="568a5-121">Then when a request is being handled, any properties a convention added or modified can be accessed through `ActionContext.ActionDescriptor.Properties`.</span></span> <span data-ttu-id="568a5-122">Za pomocą właściwości to doskonały sposób, aby skonfigurować swoje filtry, integratorów modeli, itp. na podstawie poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="568a5-122">Using properties is a great way to configure your filters, model binders, etc. on a per-action basis.</span></span>

> [!NOTE]
> <span data-ttu-id="568a5-123">`ActionDescriptor.Properties` Kolekcji nie jest bezpieczny wątkowo (w przypadku zapisów), po zakończeniu uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="568a5-123">The `ActionDescriptor.Properties` collection isn't thread safe (for writes) once app startup has finished.</span></span> <span data-ttu-id="568a5-124">Konwencje są najlepszym sposobem bezpiecznego dodania danych do tej kolekcji.</span><span class="sxs-lookup"><span data-stu-id="568a5-124">Conventions are the best way to safely add data to this collection.</span></span>

### <a name="iapplicationmodelprovider"></a><span data-ttu-id="568a5-125">IApplicationModelProvider</span><span class="sxs-lookup"><span data-stu-id="568a5-125">IApplicationModelProvider</span></span>

<span data-ttu-id="568a5-126">Platforma ASP.NET Core MVC ładuje model aplikacji przy użyciu wzorca dostawcy, zdefiniowane przez [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="568a5-126">ASP.NET Core MVC loads the application model using a provider pattern, defined by the [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface.</span></span> <span data-ttu-id="568a5-127">W tej sekcji omówiono niektóre szczegóły wewnętrznej implementacji dotyczące tej funkcji dostawcy.</span><span class="sxs-lookup"><span data-stu-id="568a5-127">This section covers some of the internal implementation details of how this provider functions.</span></span> <span data-ttu-id="568a5-128">To jest temat zaawansowana — większość aplikacji, które korzystają z modelu aplikacji to zrobić poprzez współdziałanie z Konwencji.</span><span class="sxs-lookup"><span data-stu-id="568a5-128">This is an advanced topic - most apps that leverage the application model should do so by working with conventions.</span></span>

<span data-ttu-id="568a5-129">Implementacje `IApplicationModelProvider` interfejsu "wrap", z każdym wywołaniem implementacji `OnProvidersExecuting` w kolejności rosnącej na podstawie jego `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="568a5-129">Implementations of the `IApplicationModelProvider` interface "wrap" one another, with each implementation calling `OnProvidersExecuting` in ascending order based on its `Order` property.</span></span> <span data-ttu-id="568a5-130">`OnProvidersExecuted` Zostanie następnie wywołana metoda w odwrotnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="568a5-130">The `OnProvidersExecuted` method is then called in reverse order.</span></span> <span data-ttu-id="568a5-131">Struktura zdefiniowano wielu dostawców:</span><span class="sxs-lookup"><span data-stu-id="568a5-131">The framework defines several providers:</span></span>

<span data-ttu-id="568a5-132">Pierwszy (`Order=-1000`):</span><span class="sxs-lookup"><span data-stu-id="568a5-132">First (`Order=-1000`):</span></span>

* [`DefaultApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

<span data-ttu-id="568a5-133">Następnie (`Order=-990`):</span><span class="sxs-lookup"><span data-stu-id="568a5-133">Then (`Order=-990`):</span></span>

* [`AuthorizationApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> <span data-ttu-id="568a5-134">Kolejność, w których dwóch dostawców o tej samej wartości `Order` są nazywane jest nieokreślone i dlatego nie powinny być powoływane.</span><span class="sxs-lookup"><span data-stu-id="568a5-134">The order in which two providers with the same value for `Order` are called is undefined, and therefore shouldn't be relied upon.</span></span>

> [!NOTE]
> <span data-ttu-id="568a5-135">`IApplicationModelProvider` to zaawansowane pojęcia dla autorów framework rozszerzyć.</span><span class="sxs-lookup"><span data-stu-id="568a5-135">`IApplicationModelProvider` is an advanced concept for framework authors to extend.</span></span> <span data-ttu-id="568a5-136">Ogólnie rzecz biorąc aplikacje powinny używać konwencji i platform należy używać dostawcy.</span><span class="sxs-lookup"><span data-stu-id="568a5-136">In general, apps should use conventions and frameworks should use providers.</span></span> <span data-ttu-id="568a5-137">Klucza różnica polega na tym, że dostawcy są zawsze uruchamiane przed Konwencji.</span><span class="sxs-lookup"><span data-stu-id="568a5-137">The key distinction is that providers always run before conventions.</span></span>

<span data-ttu-id="568a5-138">`DefaultApplicationModelProvider` Ustanawia wiele zachowania domyślne używane przez platformę ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="568a5-138">The `DefaultApplicationModelProvider` establishes many of the default behaviors used by ASP.NET Core MVC.</span></span> <span data-ttu-id="568a5-139">Jej obowiązki obejmują:</span><span class="sxs-lookup"><span data-stu-id="568a5-139">Its responsibilities include:</span></span>

* <span data-ttu-id="568a5-140">Dodawanie filtrów globalnych do kontekstu</span><span class="sxs-lookup"><span data-stu-id="568a5-140">Adding global filters to the context</span></span>
* <span data-ttu-id="568a5-141">Dodanie kontrolerów do kontekstu</span><span class="sxs-lookup"><span data-stu-id="568a5-141">Adding controllers to the context</span></span>
* <span data-ttu-id="568a5-142">Dodawanie metod publicznych kontrolera jako akcje</span><span class="sxs-lookup"><span data-stu-id="568a5-142">Adding public controller methods as actions</span></span>
* <span data-ttu-id="568a5-143">Dodawanie parametrów metody akcji dla kontekstu</span><span class="sxs-lookup"><span data-stu-id="568a5-143">Adding action method parameters to the context</span></span>
* <span data-ttu-id="568a5-144">Stosowanie się trasy i innych atrybutów</span><span class="sxs-lookup"><span data-stu-id="568a5-144">Applying route and other attributes</span></span>

<span data-ttu-id="568a5-145">Niektóre z wbudowanymi zachowaniami są implementowane przez `DefaultApplicationModelProvider`.</span><span class="sxs-lookup"><span data-stu-id="568a5-145">Some built-in behaviors are implemented by the `DefaultApplicationModelProvider`.</span></span> <span data-ttu-id="568a5-146">Ten dostawca jest odpowiedzialny za konstruowanie [ `ControllerModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), który z kolei odwołuje się do [ `ActionModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), i [ `ParameterModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) wystąpień.</span><span class="sxs-lookup"><span data-stu-id="568a5-146">This provider is responsible for constructing the [`ControllerModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), which in turn references [`ActionModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [`PropertyModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), and [`ParameterModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instances.</span></span> <span data-ttu-id="568a5-147">`DefaultApplicationModelProvider` Klasa to szczegół implementacji wewnętrzną strukturę, który może i zmieni się w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="568a5-147">The `DefaultApplicationModelProvider` class is an internal framework implementation detail that can and will change in the future.</span></span> 

<span data-ttu-id="568a5-148">`AuthorizationApplicationModelProvider` Odpowiada za stosowanie zachowania związanego z `AuthorizeFilter` i `AllowAnonymousFilter` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="568a5-148">The `AuthorizationApplicationModelProvider` is responsible for applying the behavior associated with the `AuthorizeFilter` and `AllowAnonymousFilter` attributes.</span></span> <span data-ttu-id="568a5-149">[Dowiedz się więcej o tych atrybutów](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="568a5-149">[Learn more about these attributes](xref:security/authorization/simple).</span></span>

<span data-ttu-id="568a5-150">`CorsApplicationModelProvider` Implementuje zachowania związanego z `IEnableCorsAttribute` i `IDisableCorsAttribute`i `DisableCorsAuthorizationFilter`.</span><span class="sxs-lookup"><span data-stu-id="568a5-150">The `CorsApplicationModelProvider` implements behavior associated with the `IEnableCorsAttribute` and `IDisableCorsAttribute`, and the `DisableCorsAuthorizationFilter`.</span></span> <span data-ttu-id="568a5-151">[Dowiedz się więcej na temat mechanizmu CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="568a5-151">[Learn more about CORS](xref:security/cors).</span></span>

## <a name="conventions"></a><span data-ttu-id="568a5-152">Konwencje</span><span class="sxs-lookup"><span data-stu-id="568a5-152">Conventions</span></span>

<span data-ttu-id="568a5-153">Model aplikacji definiuje abstrakcje Konwencji, zapewniających prostszy sposób, aby dostosować zachowanie modeli niż zastępując cały model lub dostawcy.</span><span class="sxs-lookup"><span data-stu-id="568a5-153">The application model defines convention abstractions that provide a simpler way to customize the behavior of the models than overriding the entire model or provider.</span></span> <span data-ttu-id="568a5-154">Te obiekty abstrakcyjne są zalecanym sposobem modyfikowania zachowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="568a5-154">These abstractions are the recommended way to modify your app's behavior.</span></span> <span data-ttu-id="568a5-155">Konwencje umożliwiają pisanie kodu, która dynamicznie będzie miała zastosowanie dostosowań.</span><span class="sxs-lookup"><span data-stu-id="568a5-155">Conventions provide a way for you to write code that will dynamically apply customizations.</span></span> <span data-ttu-id="568a5-156">Gdy [filtry](xref:mvc/controllers/filters) oferują możliwość modyfikowania zachowania struktury, dostosowania pozwalają na kontrolę, jak razem przewodowej całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="568a5-156">While [filters](xref:mvc/controllers/filters) provide a means of modifying the framework's behavior, customizations let you control how the whole app is wired together.</span></span>

<span data-ttu-id="568a5-157">Dostępne są następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="568a5-157">The following conventions are available:</span></span>

* [`IApplicationModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

<span data-ttu-id="568a5-158">Konwencje są stosowane przez dodanie ich do MVC opcji lub poprzez implementację `Attribute`s, stosując je do kontrolerów, akcji lub parametry akcji (podobnie jak [ `Filters` ](xref:mvc/controllers/filters)).</span><span class="sxs-lookup"><span data-stu-id="568a5-158">Conventions are applied by adding them to MVC options or by implementing `Attribute`s and applying them to controllers, actions, or action parameters (similar to [`Filters`](xref:mvc/controllers/filters)).</span></span> <span data-ttu-id="568a5-159">W przeciwieństwie do filtrów konwencje są wykonywane tylko podczas uruchamiania aplikacji, nie jako część każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="568a5-159">Unlike filters, conventions are only executed when the app is starting, not as part of each request.</span></span>

### <a name="sample-modifying-the-applicationmodel"></a><span data-ttu-id="568a5-160">Przykład: Modyfikowanie ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="568a5-160">Sample: Modifying the ApplicationModel</span></span>

<span data-ttu-id="568a5-161">Następująca Konwencja umożliwia dodawanie właściwości do modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="568a5-161">The following convention is used to add a property to the application model.</span></span> 

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

<span data-ttu-id="568a5-162">Konwencje modelu aplikacji są stosowane jako opcje po dodaniu MVC `ConfigureServices` w `Startup`.</span><span class="sxs-lookup"><span data-stu-id="568a5-162">Application model conventions are applied as options when MVC is added in `ConfigureServices` in `Startup`.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

<span data-ttu-id="568a5-163">Właściwości są dostępne z `ActionDescriptor` kolekcji właściwości w ramach akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="568a5-163">Properties are accessible from the `ActionDescriptor` properties collection within controller actions:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a><span data-ttu-id="568a5-164">Przykład: Modyfikowanie opisu ControllerModel</span><span class="sxs-lookup"><span data-stu-id="568a5-164">Sample: Modifying the ControllerModel Description</span></span>

<span data-ttu-id="568a5-165">Co w poprzednim przykładzie można także modyfikować aby uwzględnić właściwości niestandardowe modelu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="568a5-165">As in the previous example, the controller model can also be modified to include custom properties.</span></span> <span data-ttu-id="568a5-166">To spowoduje zastąpienie istniejących właściwości o takiej samej nazwie, określone w modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="568a5-166">These will override existing properties with the same name specified in the application model.</span></span> <span data-ttu-id="568a5-167">Następujący atrybut Konwencji dodano opis na poziomie kontrolera:</span><span class="sxs-lookup"><span data-stu-id="568a5-167">The following convention attribute adds a description at the controller level:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

<span data-ttu-id="568a5-168">Ta konwencja jest stosowana jako atrybut na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="568a5-168">This convention is applied as an attribute on a controller.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

<span data-ttu-id="568a5-169">Właściwość "description" jest dostępny w taki sam sposób jak w poprzednich przykładach.</span><span class="sxs-lookup"><span data-stu-id="568a5-169">The "description" property is accessed in the same manner as in previous examples.</span></span>

### <a name="sample-modifying-the-actionmodel-description"></a><span data-ttu-id="568a5-170">Przykład: Modyfikowanie opisu ActionModel</span><span class="sxs-lookup"><span data-stu-id="568a5-170">Sample: Modifying the ActionModel Description</span></span>

<span data-ttu-id="568a5-171">Konwencja oddzielny atrybut można zastosować do poszczególnych czynności, zastępowanie zachowania już zastosowana na poziomie aplikacji lub kontrolera.</span><span class="sxs-lookup"><span data-stu-id="568a5-171">A separate attribute convention can be applied to individual actions, overriding behavior already applied at the application or controller level.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

<span data-ttu-id="568a5-172">Zastosowanie do akcji w kontrolerze w poprzednim przykładzie pokazano, jak zastępuje ona Konwencji poziomie kontrolera:</span><span class="sxs-lookup"><span data-stu-id="568a5-172">Applying this to an action within the previous example's controller demonstrates how it overrides the controller-level convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a><span data-ttu-id="568a5-173">Przykład: Modyfikowanie ParameterModel</span><span class="sxs-lookup"><span data-stu-id="568a5-173">Sample: Modifying the ParameterModel</span></span>

<span data-ttu-id="568a5-174">Następująca Konwencja mogą być stosowane do parametrów akcji do modyfikowania ich `BindingInfo`.</span><span class="sxs-lookup"><span data-stu-id="568a5-174">The following convention can be applied to action parameters to modify their `BindingInfo`.</span></span> <span data-ttu-id="568a5-175">Następująca Konwencja wymaga, aby parametr parametru trasy; inne potencjalne powiązanie źródeł (na przykład wartości ciągu zapytania), są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="568a5-175">The following convention requires that the parameter be a route parameter; other potential binding sources (such as query string values) are ignored.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

<span data-ttu-id="568a5-176">Ten atrybut można stosować do dowolnej parametr akcji:</span><span class="sxs-lookup"><span data-stu-id="568a5-176">The attribute may be applied to any action parameter:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a><span data-ttu-id="568a5-177">Przykład: Modyfikowanie nazwy ActionModel</span><span class="sxs-lookup"><span data-stu-id="568a5-177">Sample: Modifying the ActionModel Name</span></span>

<span data-ttu-id="568a5-178">Modyfikuje następującej konwencji `ActionModel` można zaktualizować *nazwa* akcji, do którego jest stosowany.</span><span class="sxs-lookup"><span data-stu-id="568a5-178">The following convention modifies the `ActionModel` to update the *name* of the action to which it's applied.</span></span> <span data-ttu-id="568a5-179">Nowa nazwa jest podać jako parametr do atrybutu.</span><span class="sxs-lookup"><span data-stu-id="568a5-179">The new name is provided as a parameter to the attribute.</span></span> <span data-ttu-id="568a5-180">Ta nowa nazwa jest używany przez routingu, więc będzie miało wpływ na trasy, używany w celu osiągnięcia tą metodą akcji.</span><span class="sxs-lookup"><span data-stu-id="568a5-180">This new name is used by routing, so it will affect the route used to reach this action method.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

<span data-ttu-id="568a5-181">Ten atrybut jest stosowany do metody akcji w `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="568a5-181">This attribute is applied to an action method in the `HomeController`:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

<span data-ttu-id="568a5-182">Mimo że w nazwie metody istotna jest `SomeName`, atrybut zastępuje Konwencji MVC przy użyciu nazwy metody i zastępuje Nazwa akcji z `MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="568a5-182">Even though the method name is `SomeName`, the attribute overrides the MVC convention of using the method name and replaces the action name with `MyCoolAction`.</span></span> <span data-ttu-id="568a5-183">W związku z tym, trasy używany w celu osiągnięcia ta akcja jest `/Home/MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="568a5-183">Thus, the route used to reach this action is `/Home/MyCoolAction`.</span></span>

> [!NOTE]
> <span data-ttu-id="568a5-184">W tym przykładzie jest zasadniczo taki sam, jak przy użyciu wbudowanych [ActionName](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="568a5-184">This example is essentially the same as using the built-in [ActionName](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) attribute.</span></span>

### <a name="sample-custom-routing-convention"></a><span data-ttu-id="568a5-185">Przykład: Niestandardowe Konwencji routingu</span><span class="sxs-lookup"><span data-stu-id="568a5-185">Sample: Custom Routing Convention</span></span>

<span data-ttu-id="568a5-186">Możesz użyć `IApplicationModelConvention` dostosować działa jak routingu.</span><span class="sxs-lookup"><span data-stu-id="568a5-186">You can use an `IApplicationModelConvention` to customize how routing works.</span></span> <span data-ttu-id="568a5-187">Na przykład następująca Konwencja będą zawierały kontrolerami przestrzenie nazw do ich tras, zastępując `.` w przestrzeni nazw za pomocą `/` dla trasy:</span><span class="sxs-lookup"><span data-stu-id="568a5-187">For example, the following convention will incorporate Controllers' namespaces into their routes, replacing `.` in the namespace with `/` in the route:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

<span data-ttu-id="568a5-188">Konwencji jest dodawany jako opcję uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="568a5-188">The convention is added as an option in Startup.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> <span data-ttu-id="568a5-189">Możesz dodać konwencje do Twojej [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) , uzyskując dostęp do `MvcOptions` przy użyciu `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span><span class="sxs-lookup"><span data-stu-id="568a5-189">You can add conventions to your [middleware](xref:fundamentals/middleware/index) by accessing `MvcOptions` using `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span></span>

<span data-ttu-id="568a5-190">Ten przykład dotyczy tę Konwencję tras, które nie korzystają z trasami atrybutów, których kontroler ma "Namespace" w nazwie.</span><span class="sxs-lookup"><span data-stu-id="568a5-190">This sample applies this convention to routes that are not using attribute routing where the controller has  "Namespace" in its name.</span></span> <span data-ttu-id="568a5-191">Następujący kontroler pokazuje tę Konwencję:</span><span class="sxs-lookup"><span data-stu-id="568a5-191">The following controller demonstrates this convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a><span data-ttu-id="568a5-192">Sposób użycia modelu aplikacji w WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="568a5-192">Application Model Usage in WebApiCompatShim</span></span>

<span data-ttu-id="568a5-193">Platforma ASP.NET Core MVC korzysta z innego zestawu Konwencji z wzorca ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="568a5-193">ASP.NET Core MVC uses a different set of conventions from ASP.NET Web API 2.</span></span> <span data-ttu-id="568a5-194">Za pomocą Konwencji niestandardowych, można zmodyfikować zachowanie aplikację ASP.NET Core MVC, aby były zgodne z tą aplikacją interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="568a5-194">Using custom conventions, you can modify an ASP.NET Core MVC app's behavior to be consistent with that of a Web API app.</span></span> <span data-ttu-id="568a5-195">Microsoft dostarczany [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specjalnie do tego celu.</span><span class="sxs-lookup"><span data-stu-id="568a5-195">Microsoft ships the [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specifically for this purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="568a5-196">Dowiedz się więcej o [migracji z interfejsu API sieci Web platformy ASP.NET](xref:migration/webapi).</span><span class="sxs-lookup"><span data-stu-id="568a5-196">Learn more about [migration from ASP.NET Web API](xref:migration/webapi).</span></span>

<span data-ttu-id="568a5-197">Aby użyć podkładek zgodności interfejsu API sieci Web, należy dodać pakiet do projektu, a następnie dodaj Konwencji do MVC, wywołując `AddWebApiConventions` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="568a5-197">To use the Web API Compatibility Shim, you need to add the package to your project and then add the conventions to MVC by calling `AddWebApiConventions` in `Startup`:</span></span>

```csharp
services.AddMvc().AddWebApiConventions();
```

<span data-ttu-id="568a5-198">Konwencje podane według podkładki są stosowane tylko do części aplikacji, których niektóre atrybuty zastosowanych do nich.</span><span class="sxs-lookup"><span data-stu-id="568a5-198">The conventions provided by the shim are only applied to parts of the app that have had certain attributes applied to them.</span></span> <span data-ttu-id="568a5-199">Następujące cztery atrybuty są używane do formantu, co kontrolery powinny mieć ich konwencje zmodyfikowane przez konwencje podkładek:</span><span class="sxs-lookup"><span data-stu-id="568a5-199">The following four attributes are used to control which controllers should have their conventions modified by the shim's conventions:</span></span>

* [<span data-ttu-id="568a5-200">UseWebApiActionConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="568a5-200">UseWebApiActionConventionsAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [<span data-ttu-id="568a5-201">UseWebApiOverloadingAttribute</span><span class="sxs-lookup"><span data-stu-id="568a5-201">UseWebApiOverloadingAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [<span data-ttu-id="568a5-202">UseWebApiParameterConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="568a5-202">UseWebApiParameterConventionsAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [<span data-ttu-id="568a5-203">UseWebApiRoutesAttribute</span><span class="sxs-lookup"><span data-stu-id="568a5-203">UseWebApiRoutesAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a><span data-ttu-id="568a5-204">Konwencje akcji</span><span class="sxs-lookup"><span data-stu-id="568a5-204">Action Conventions</span></span>

<span data-ttu-id="568a5-205">`UseWebApiActionConventionsAttribute` Służy do mapowania metoda HTTP do akcji na podstawie ich nazwy (na przykład `Get` mapującej do `HttpGet`).</span><span class="sxs-lookup"><span data-stu-id="568a5-205">The `UseWebApiActionConventionsAttribute` is used to map the HTTP method to actions based on their name (for instance, `Get` would map to `HttpGet`).</span></span> <span data-ttu-id="568a5-206">Dotyczy tylko akcje, które nie korzystają z trasami atrybutów.</span><span class="sxs-lookup"><span data-stu-id="568a5-206">It only applies to actions that don't use attribute routing.</span></span>

### <a name="overloading"></a><span data-ttu-id="568a5-207">Przeciążenie</span><span class="sxs-lookup"><span data-stu-id="568a5-207">Overloading</span></span>

<span data-ttu-id="568a5-208">`UseWebApiOverloadingAttribute` Służy do stosowania `WebApiOverloadingApplicationModelConvention` Konwencji.</span><span class="sxs-lookup"><span data-stu-id="568a5-208">The `UseWebApiOverloadingAttribute` is used to apply the `WebApiOverloadingApplicationModelConvention` convention.</span></span> <span data-ttu-id="568a5-209">Dodaje tę konwencję `OverloadActionConstraint` do procesu wyboru akcji, co ogranicza akcji kandydata do tych, dla których żądanie spełnia wszystkie parametry — opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="568a5-209">This convention adds an `OverloadActionConstraint` to the action selection process, which limits candidate actions to those for which the request satisfies all non-optional parameters.</span></span>

### <a name="parameter-conventions"></a><span data-ttu-id="568a5-210">Konwencje parametru</span><span class="sxs-lookup"><span data-stu-id="568a5-210">Parameter Conventions</span></span>

<span data-ttu-id="568a5-211">`UseWebApiParameterConventionsAttribute` Służy do stosowania `WebApiParameterConventionsApplicationModelConvention` Konwencji akcji.</span><span class="sxs-lookup"><span data-stu-id="568a5-211">The `UseWebApiParameterConventionsAttribute` is used to apply the `WebApiParameterConventionsApplicationModelConvention` action convention.</span></span> <span data-ttu-id="568a5-212">Ta konwencja określa, czy proste typy używane jako parametry akcji są powiązane z identyfikatora URI domyślnie, gdy typy złożone są powiązane z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="568a5-212">This convention specifies that simple types used as action parameters are bound from the URI by default, while complex types are bound from the request body.</span></span>

### <a name="routes"></a><span data-ttu-id="568a5-213">Trasy</span><span class="sxs-lookup"><span data-stu-id="568a5-213">Routes</span></span>

<span data-ttu-id="568a5-214">`UseWebApiRoutesAttribute` Formanty czy `WebApiApplicationModelConvention` jest stosowana Konwencja kontrolera.</span><span class="sxs-lookup"><span data-stu-id="568a5-214">The `UseWebApiRoutesAttribute` controls whether the `WebApiApplicationModelConvention` controller convention is applied.</span></span> <span data-ttu-id="568a5-215">Po włączeniu ta Konwencja umożliwia obsługę [obszarów](xref:mvc/controllers/areas) do trasy.</span><span class="sxs-lookup"><span data-stu-id="568a5-215">When enabled, this convention is used to add support for [areas](xref:mvc/controllers/areas) to the route.</span></span>

<span data-ttu-id="568a5-216">Oprócz zestaw Konwencji, zawiera pakiet zgodności `System.Web.Http.ApiController` podstawowej klasy, która zastępuje ten, który został udostępniony przez interfejs API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="568a5-216">In addition to a set of conventions, the compatibility package includes a `System.Web.Http.ApiController` base class that replaces the one provided by Web API.</span></span> <span data-ttu-id="568a5-217">Dzięki temu kontrolerach napisanych dla interfejsu API sieci Web i dziedziczenie z jego `ApiController` działać zgodnie z zostały zaprojektowane, podczas uruchamiania na platformie ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="568a5-217">This allows your controllers written for Web API and inheriting from its `ApiController` to work as they were designed, while running on ASP.NET Core MVC.</span></span> <span data-ttu-id="568a5-218">Ta klasa kontrolera podstawowego zostanie nadany ze wszystkimi `UseWebApi*` atrybuty wymienione powyżej.</span><span class="sxs-lookup"><span data-stu-id="568a5-218">This base controller class is decorated with all of the `UseWebApi*` attributes listed above.</span></span> <span data-ttu-id="568a5-219">`ApiController` Udostępnia właściwości, metody i typy wyników, które są zgodne z tych dostępnych w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="568a5-219">The `ApiController` exposes properties, methods, and result types that are compatible with those found in Web API.</span></span>

## <a name="using-apiexplorer-to-document-your-app"></a><span data-ttu-id="568a5-220">Dokumentów aplikacji za pomocą ApiExplorer</span><span class="sxs-lookup"><span data-stu-id="568a5-220">Using ApiExplorer to Document Your App</span></span>

<span data-ttu-id="568a5-221">Udostępnia model aplikacji [ `ApiExplorer` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) właściwości na każdym poziomie, który może służyć do przechodzenia struktury aplikacji.</span><span class="sxs-lookup"><span data-stu-id="568a5-221">The application model exposes an [`ApiExplorer`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) property at each level that can be used to traverse the app's structure.</span></span> <span data-ttu-id="568a5-222">Może to być używane do [generowania strony pomocy dla interfejsów API sieci Web przy użyciu narzędzia, takie jak Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="568a5-222">This can be used to [generate help pages for your Web APIs using tools like Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="568a5-223">`ApiExplorer` Ujawnia właściwość `IsVisible` właściwości, które można ustawić, aby określić części modelu aplikacji, które powinny zostać ujawnione.</span><span class="sxs-lookup"><span data-stu-id="568a5-223">The `ApiExplorer` property exposes an `IsVisible` property that can be set to specify which parts of your app's model should be exposed.</span></span> <span data-ttu-id="568a5-224">Można to ustawienie można skonfigurować przy użyciu Konwencji:</span><span class="sxs-lookup"><span data-stu-id="568a5-224">You can configure this setting using a convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

<span data-ttu-id="568a5-225">Korzystając z tego podejścia (dodatkowe konwencje i w razie potrzeby), można włączyć lub wyłączyć widoczność interfejsu API na dowolnym poziomie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="568a5-225">Using this approach (and additional conventions if required), you can enable or disable API visibility at any level within your app.</span></span> 
