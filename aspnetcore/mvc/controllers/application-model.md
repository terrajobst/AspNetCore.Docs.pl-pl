---
title: Pracuj z modelem aplikacji w ASP.NET Core
author: ardalis
description: Dowiedz się, jak czytać i manipulować modelem aplikacji, aby modyfikować sposób zachowania elementów MVC w ASP.NET Core.
ms.author: riande
ms.date: 12/05/2019
uid: mvc/controllers/application-model
ms.openlocfilehash: 4b6c978e5752eb320412a1c204df8e3d288fe4a1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666433"
---
# <a name="work-with-the-application-model-in-aspnet-core"></a><span data-ttu-id="bbfdc-103">Pracuj z modelem aplikacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bbfdc-103">Work with the application model in ASP.NET Core</span></span>

<span data-ttu-id="bbfdc-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="bbfdc-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="bbfdc-105">ASP.NET Core MVC definiuje *model aplikacji* reprezentujący składniki aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-105">ASP.NET Core MVC defines an *application model* representing the components of an MVC app.</span></span> <span data-ttu-id="bbfdc-106">Można odczytywać i manipulować tym modelem, aby modyfikować sposób zachowania elementów MVC.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-106">You can read and manipulate this model to modify how MVC elements behave.</span></span> <span data-ttu-id="bbfdc-107">Domyślnie MVC stosuje pewne konwencje, aby określić, które klasy są uważane za kontrolery, które metody dla tych klas są akcjami oraz jak działają parametry i Routing.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-107">By default, MVC follows certain conventions to determine which classes are considered to be controllers, which methods on those classes are actions, and how parameters and routing behave.</span></span> <span data-ttu-id="bbfdc-108">Takie zachowanie można dostosować do potrzeb aplikacji, tworząc własne konwencje i stosując je globalnie lub jako atrybuty.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-108">You can customize this behavior to suit your app's needs by creating your own conventions and applying them globally or as attributes.</span></span>

## <a name="models-and-providers"></a><span data-ttu-id="bbfdc-109">Modele i dostawcy</span><span class="sxs-lookup"><span data-stu-id="bbfdc-109">Models and Providers</span></span>

<span data-ttu-id="bbfdc-110">Model aplikacji ASP.NET Core MVC obejmuje zarówno interfejsy abstrakcyjne, jak i klasy konkretnych implementacji, które opisują aplikację MVC.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-110">The ASP.NET Core MVC application model include both abstract interfaces and concrete implementation classes that describe an MVC application.</span></span> <span data-ttu-id="bbfdc-111">Ten model jest wynikiem odnajdywania kontrolerów aplikacji, akcji, parametrów akcji, tras i filtrów zgodnie z konwencjami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-111">This model is the result of MVC discovering the app's controllers, actions, action parameters, routes, and filters according to default conventions.</span></span> <span data-ttu-id="bbfdc-112">Pracując z modelem aplikacji, można zmodyfikować aplikację, tak aby była zgodna z innymi konwencjami z domyślnego zachowania MVC.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-112">By working with the application model, you can modify your app to follow different conventions from the default MVC behavior.</span></span> <span data-ttu-id="bbfdc-113">Parametry, nazwy, trasy i filtry są używane jako dane konfiguracji dla akcji i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-113">The parameters, names, routes, and filters are all used as configuration data for actions and controllers.</span></span>

<span data-ttu-id="bbfdc-114">Model aplikacji ASP.NET Core MVC ma następującą strukturę:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-114">The ASP.NET Core MVC Application Model has the following structure:</span></span>

* <span data-ttu-id="bbfdc-115">ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="bbfdc-115">ApplicationModel</span></span>
  * <span data-ttu-id="bbfdc-116">Kontrolery (ControllerModel)</span><span class="sxs-lookup"><span data-stu-id="bbfdc-116">Controllers (ControllerModel)</span></span>
    * <span data-ttu-id="bbfdc-117">Akcje (ActionModel)</span><span class="sxs-lookup"><span data-stu-id="bbfdc-117">Actions (ActionModel)</span></span>
      * <span data-ttu-id="bbfdc-118">Parametry (ParameterModel)</span><span class="sxs-lookup"><span data-stu-id="bbfdc-118">Parameters (ParameterModel)</span></span>

<span data-ttu-id="bbfdc-119">Każdy poziom modelu ma dostęp do typowej kolekcji `Properties`, a niższe poziomy mogą uzyskać dostęp do wartości właściwości ustawionych przez wyższe poziomy w hierarchii i zastępować je.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-119">Each level of the model has access to a common `Properties` collection, and lower levels can access and overwrite property values set by higher levels in the hierarchy.</span></span> <span data-ttu-id="bbfdc-120">Właściwości są utrwalane w `ActionDescriptor.Properties` podczas tworzenia akcji.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-120">The properties are persisted to the `ActionDescriptor.Properties` when the actions are created.</span></span> <span data-ttu-id="bbfdc-121">Następnie, gdy żądanie jest obsługiwane, można uzyskać dostęp do wszystkich właściwości dodanej lub zmodyfikowanej Konwencji za pomocą `ActionContext.ActionDescriptor.Properties`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-121">Then when a request is being handled, any properties a convention added or modified can be accessed through `ActionContext.ActionDescriptor.Properties`.</span></span> <span data-ttu-id="bbfdc-122">Korzystanie z właściwości to doskonały sposób konfigurowania filtrów, powiązań modelu itp. na podstawie akcji.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-122">Using properties is a great way to configure your filters, model binders, etc. on a per-action basis.</span></span>

> [!NOTE]
> <span data-ttu-id="bbfdc-123">Kolekcja `ActionDescriptor.Properties` nie jest bezpieczna wątkowo (w przypadku zapisów) po zakończeniu uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-123">The `ActionDescriptor.Properties` collection isn't thread safe (for writes) once app startup has finished.</span></span> <span data-ttu-id="bbfdc-124">Konwencje są najlepszym sposobem bezpiecznego dodawania danych do tej kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-124">Conventions are the best way to safely add data to this collection.</span></span>

### <a name="iapplicationmodelprovider"></a><span data-ttu-id="bbfdc-125">IApplicationModelProvider</span><span class="sxs-lookup"><span data-stu-id="bbfdc-125">IApplicationModelProvider</span></span>

<span data-ttu-id="bbfdc-126">ASP.NET Core MVC ładuje model aplikacji przy użyciu wzorca dostawcy zdefiniowanego przez interfejs [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) .</span><span class="sxs-lookup"><span data-stu-id="bbfdc-126">ASP.NET Core MVC loads the application model using a provider pattern, defined by the [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface.</span></span> <span data-ttu-id="bbfdc-127">W tej sekcji omówiono niektóre wewnętrzne szczegóły implementacji działania tego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-127">This section covers some of the internal implementation details of how this provider functions.</span></span> <span data-ttu-id="bbfdc-128">Jest to zaawansowany temat-większość aplikacji, które wykorzystują model aplikacji, należy to zrobić przez pracę z konwencjami.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-128">This is an advanced topic - most apps that leverage the application model should do so by working with conventions.</span></span>

<span data-ttu-id="bbfdc-129">Implementacje interfejsu `IApplicationModelProvider` "zawijają", przy czym każda implementacja wywołuje `OnProvidersExecuting` w kolejności rosnącej na podstawie jej właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-129">Implementations of the `IApplicationModelProvider` interface "wrap" one another, with each implementation calling `OnProvidersExecuting` in ascending order based on its `Order` property.</span></span> <span data-ttu-id="bbfdc-130">Metoda `OnProvidersExecuted` jest następnie wywoływana w odwrotnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-130">The `OnProvidersExecuted` method is then called in reverse order.</span></span> <span data-ttu-id="bbfdc-131">Struktura definiuje kilku dostawców:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-131">The framework defines several providers:</span></span>

<span data-ttu-id="bbfdc-132">Pierwszy (`Order=-1000`):</span><span class="sxs-lookup"><span data-stu-id="bbfdc-132">First (`Order=-1000`):</span></span>

* [`DefaultApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

<span data-ttu-id="bbfdc-133">Następnie (`Order=-990`):</span><span class="sxs-lookup"><span data-stu-id="bbfdc-133">Then (`Order=-990`):</span></span>

* [`AuthorizationApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> <span data-ttu-id="bbfdc-134">Kolejność, w której są dwa dostawcy o tej samej wartości dla `Order` jest niezdefiniowana i dlatego nie powinna być używana.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-134">The order in which two providers with the same value for `Order` are called is undefined, and therefore shouldn't be relied upon.</span></span>

> [!NOTE]
> <span data-ttu-id="bbfdc-135">`IApplicationModelProvider` jest zaawansowaną koncepcją dla autorów struktury, która ma zostać rozszerzona.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-135">`IApplicationModelProvider` is an advanced concept for framework authors to extend.</span></span> <span data-ttu-id="bbfdc-136">Ogólnie rzecz biorąc aplikacje powinny używać konwencji i struktur, powinny używać dostawców.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-136">In general, apps should use conventions and frameworks should use providers.</span></span> <span data-ttu-id="bbfdc-137">Odrębność klucza polega na tym, że dostawcy zawsze są uruchamiani przed konwencjami.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-137">The key distinction is that providers always run before conventions.</span></span>

<span data-ttu-id="bbfdc-138">`DefaultApplicationModelProvider` określa wiele zachowań domyślnych używanych przez ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-138">The `DefaultApplicationModelProvider` establishes many of the default behaviors used by ASP.NET Core MVC.</span></span> <span data-ttu-id="bbfdc-139">Jej obowiązki obejmują:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-139">Its responsibilities include:</span></span>

* <span data-ttu-id="bbfdc-140">Dodawanie filtrów globalnych do kontekstu</span><span class="sxs-lookup"><span data-stu-id="bbfdc-140">Adding global filters to the context</span></span>
* <span data-ttu-id="bbfdc-141">Dodawanie kontrolerów do kontekstu</span><span class="sxs-lookup"><span data-stu-id="bbfdc-141">Adding controllers to the context</span></span>
* <span data-ttu-id="bbfdc-142">Dodawanie metod kontrolera publicznego jako akcji</span><span class="sxs-lookup"><span data-stu-id="bbfdc-142">Adding public controller methods as actions</span></span>
* <span data-ttu-id="bbfdc-143">Dodawanie parametrów metody akcji do kontekstu</span><span class="sxs-lookup"><span data-stu-id="bbfdc-143">Adding action method parameters to the context</span></span>
* <span data-ttu-id="bbfdc-144">Stosowanie trasy i innych atrybutów</span><span class="sxs-lookup"><span data-stu-id="bbfdc-144">Applying route and other attributes</span></span>

<span data-ttu-id="bbfdc-145">Niektóre wbudowane zachowania są implementowane przez `DefaultApplicationModelProvider`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-145">Some built-in behaviors are implemented by the `DefaultApplicationModelProvider`.</span></span> <span data-ttu-id="bbfdc-146">Ten dostawca jest odpowiedzialny za konstruowanie [`ControllerModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), które z kolei odwołują się do [`ActionModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel), [`PropertyModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel)i [`ParameterModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel) wystąpień.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-146">This provider is responsible for constructing the [`ControllerModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), which in turn references [`ActionModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel), [`PropertyModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), and [`ParameterModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel) instances.</span></span> <span data-ttu-id="bbfdc-147">Klasa `DefaultApplicationModelProvider` jest wewnętrznymi szczegółami implementacji platformy, które mogą i zmienią się w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-147">The `DefaultApplicationModelProvider` class is an internal framework implementation detail that can and will change in the future.</span></span> 

<span data-ttu-id="bbfdc-148">`AuthorizationApplicationModelProvider` jest odpowiedzialna za stosowanie zachowania związanego z atrybutami `AuthorizeFilter` i `AllowAnonymousFilter`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-148">The `AuthorizationApplicationModelProvider` is responsible for applying the behavior associated with the `AuthorizeFilter` and `AllowAnonymousFilter` attributes.</span></span> <span data-ttu-id="bbfdc-149">[Dowiedz się więcej na temat tych atrybutów](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="bbfdc-149">[Learn more about these attributes](xref:security/authorization/simple).</span></span>

<span data-ttu-id="bbfdc-150">`CorsApplicationModelProvider` implementuje zachowanie skojarzone z `IEnableCorsAttribute` i `IDisableCorsAttribute`i `DisableCorsAuthorizationFilter`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-150">The `CorsApplicationModelProvider` implements behavior associated with the `IEnableCorsAttribute` and `IDisableCorsAttribute`, and the `DisableCorsAuthorizationFilter`.</span></span> <span data-ttu-id="bbfdc-151">[Dowiedz się więcej na temat mechanizmu CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="bbfdc-151">[Learn more about CORS](xref:security/cors).</span></span>

## <a name="conventions"></a><span data-ttu-id="bbfdc-152">Konwencja</span><span class="sxs-lookup"><span data-stu-id="bbfdc-152">Conventions</span></span>

<span data-ttu-id="bbfdc-153">Model aplikacji definiuje abstrakcje Konwencji, które zapewniają prostszy sposób dostosowywania zachowania modeli niż zastępowanie całego modelu lub dostawcy.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-153">The application model defines convention abstractions that provide a simpler way to customize the behavior of the models than overriding the entire model or provider.</span></span> <span data-ttu-id="bbfdc-154">Te streszczenia są zalecanym sposobem modyfikacji zachowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-154">These abstractions are the recommended way to modify your app's behavior.</span></span> <span data-ttu-id="bbfdc-155">Konwencje umożliwiają pisanie kodu, który będzie dynamicznie stosować dostosowania.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-155">Conventions provide a way for you to write code that will dynamically apply customizations.</span></span> <span data-ttu-id="bbfdc-156">[Filtry](xref:mvc/controllers/filters) umożliwiają modyfikowanie zachowania struktury, jednak dostosowania pozwalają kontrolować, jak cała aplikacja działa razem.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-156">While [filters](xref:mvc/controllers/filters) provide a means of modifying the framework's behavior, customizations let you control how the whole app works together.</span></span>

<span data-ttu-id="bbfdc-157">Dostępne są następujące konwencje:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-157">The following conventions are available:</span></span>

* [`IApplicationModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

<span data-ttu-id="bbfdc-158">Konwencje są stosowane przez dodanie ich do opcji MVC lub przez zaimplementowanie `Attribute`s i zastosowanie ich do kontrolerów, akcji lub parametrów akcji (podobnie jak w przypadku [`Filters`](xref:mvc/controllers/filters)).</span><span class="sxs-lookup"><span data-stu-id="bbfdc-158">Conventions are applied by adding them to MVC options or by implementing `Attribute`s and applying them to controllers, actions, or action parameters (similar to [`Filters`](xref:mvc/controllers/filters)).</span></span> <span data-ttu-id="bbfdc-159">W przeciwieństwie do filtrów, konwencje są wykonywane tylko wtedy, gdy aplikacja jest uruchamiana, a nie jako część każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-159">Unlike filters, conventions are only executed when the app is starting, not as part of each request.</span></span>

### <a name="sample-modifying-the-applicationmodel"></a><span data-ttu-id="bbfdc-160">Przykład: modyfikowanie ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="bbfdc-160">Sample: Modifying the ApplicationModel</span></span>

<span data-ttu-id="bbfdc-161">Poniższa Konwencja służy do dodawania właściwości do modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-161">The following convention is used to add a property to the application model.</span></span> 

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

<span data-ttu-id="bbfdc-162">Konwencje modelu aplikacji są stosowane jako opcje po dodaniu MVC w `ConfigureServices` w `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-162">Application model conventions are applied as options when MVC is added in `ConfigureServices` in `Startup`.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

<span data-ttu-id="bbfdc-163">Właściwości są dostępne z kolekcji właściwości `ActionDescriptor` w ramach akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-163">Properties are accessible from the `ActionDescriptor` properties collection within controller actions:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a><span data-ttu-id="bbfdc-164">Przykład: Modyfikowanie opisu ControllerModel</span><span class="sxs-lookup"><span data-stu-id="bbfdc-164">Sample: Modifying the ControllerModel Description</span></span>

<span data-ttu-id="bbfdc-165">Podobnie jak w poprzednim przykładzie, model kontrolera można także zmodyfikować, aby uwzględnić właściwości niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-165">As in the previous example, the controller model can also be modified to include custom properties.</span></span> <span data-ttu-id="bbfdc-166">Zostaną one zastąpione istniejącymi właściwościami o tej samej nazwie określonej w modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-166">These will override existing properties with the same name specified in the application model.</span></span> <span data-ttu-id="bbfdc-167">Następujący atrybut Konwencji dodaje opis na poziomie kontrolera:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-167">The following convention attribute adds a description at the controller level:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

<span data-ttu-id="bbfdc-168">Ta konwencja jest stosowana jako atrybut na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-168">This convention is applied as an attribute on a controller.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

<span data-ttu-id="bbfdc-169">Właściwość "Description" jest dostępna w taki sam sposób jak w poprzednich przykładach.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-169">The "description" property is accessed in the same manner as in previous examples.</span></span>

### <a name="sample-modifying-the-actionmodel-description"></a><span data-ttu-id="bbfdc-170">Przykład: Modyfikowanie opisu ActionModel</span><span class="sxs-lookup"><span data-stu-id="bbfdc-170">Sample: Modifying the ActionModel Description</span></span>

<span data-ttu-id="bbfdc-171">Oddzielną Konwencję atrybutu można zastosować do poszczególnych akcji, zastępując zachowanie już stosowane na poziomie aplikacji lub kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-171">A separate attribute convention can be applied to individual actions, overriding behavior already applied at the application or controller level.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

<span data-ttu-id="bbfdc-172">Zastosowanie tego do akcji w ramach kontrolera poprzedniego przykładu pokazuje, jak zastępuje Konwencję poziomu kontrolera:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-172">Applying this to an action within the previous example's controller demonstrates how it overrides the controller-level convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a><span data-ttu-id="bbfdc-173">Przykład: modyfikowanie ParameterModel</span><span class="sxs-lookup"><span data-stu-id="bbfdc-173">Sample: Modifying the ParameterModel</span></span>

<span data-ttu-id="bbfdc-174">Do parametrów akcji można zastosować następujące konwencje w celu zmodyfikowania ich `BindingInfo`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-174">The following convention can be applied to action parameters to modify their `BindingInfo`.</span></span> <span data-ttu-id="bbfdc-175">Poniższa Konwencja wymaga, aby parametr był parametrem trasy; inne potencjalne źródła powiązań (takie jak wartości ciągu zapytania) są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-175">The following convention requires that the parameter be a route parameter; other potential binding sources (such as query string values) are ignored.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

<span data-ttu-id="bbfdc-176">Ten atrybut może być stosowany do dowolnego parametru akcji:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-176">The attribute may be applied to any action parameter:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a><span data-ttu-id="bbfdc-177">Przykład: modyfikowanie nazwy ActionModel</span><span class="sxs-lookup"><span data-stu-id="bbfdc-177">Sample: Modifying the ActionModel Name</span></span>

<span data-ttu-id="bbfdc-178">Poniższa Konwencja modyfikuje `ActionModel`, aby zaktualizować *nazwę* akcji, do której zastosowano.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-178">The following convention modifies the `ActionModel` to update the *name* of the action to which it's applied.</span></span> <span data-ttu-id="bbfdc-179">Nowa nazwa jest podawana jako parametr do atrybutu.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-179">The new name is provided as a parameter to the attribute.</span></span> <span data-ttu-id="bbfdc-180">Ta nowa nazwa jest używana przez Routing, więc wpłynie ona na trasę używaną do osiągnięcia tej metody akcji.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-180">This new name is used by routing, so it will affect the route used to reach this action method.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

<span data-ttu-id="bbfdc-181">Ten atrybut jest stosowany do metody akcji w `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-181">This attribute is applied to an action method in the `HomeController`:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

<span data-ttu-id="bbfdc-182">Mimo że nazwa metody jest `SomeName`, atrybut zastępuje Konwencję MVC o użyciu nazwy metody i zastępuje nazwę akcji z `MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-182">Even though the method name is `SomeName`, the attribute overrides the MVC convention of using the method name and replaces the action name with `MyCoolAction`.</span></span> <span data-ttu-id="bbfdc-183">W ten sposób trasa używana do osiągnięcia tej akcji jest `/Home/MyCoolAction`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-183">Thus, the route used to reach this action is `/Home/MyCoolAction`.</span></span>

> [!NOTE]
> <span data-ttu-id="bbfdc-184">Ten przykład jest zasadniczo taki sam jak użycie wbudowanego atrybutu [ActionName](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) .</span><span class="sxs-lookup"><span data-stu-id="bbfdc-184">This example is essentially the same as using the built-in [ActionName](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) attribute.</span></span>

### <a name="sample-custom-routing-convention"></a><span data-ttu-id="bbfdc-185">Przykład: niestandardowa Konwencja routingu</span><span class="sxs-lookup"><span data-stu-id="bbfdc-185">Sample: Custom Routing Convention</span></span>

<span data-ttu-id="bbfdc-186">Aby dostosować sposób działania routingu, można użyć `IApplicationModelConvention`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-186">You can use an `IApplicationModelConvention` to customize how routing works.</span></span> <span data-ttu-id="bbfdc-187">Na przykład następująca Konwencja obejmuje przestrzenie nazw kontrolerów, zastępując `.` w przestrzeni nazw `/` w marszrucie:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-187">For example, the following convention will incorporate Controllers' namespaces into their routes, replacing `.` in the namespace with `/` in the route:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

<span data-ttu-id="bbfdc-188">Konwencja jest dodawana jako opcja podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-188">The convention is added as an option in Startup.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> <span data-ttu-id="bbfdc-189">Możesz dodawać konwencje do [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) , uzyskując dostęp do `MvcOptions` przy użyciu `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span><span class="sxs-lookup"><span data-stu-id="bbfdc-189">You can add conventions to your [middleware](xref:fundamentals/middleware/index) by accessing `MvcOptions` using `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span></span>

<span data-ttu-id="bbfdc-190">Ten przykład dotyczy tras, które nie korzystają z routingu atrybutu, w którym kontroler ma nazwę "namespace".</span><span class="sxs-lookup"><span data-stu-id="bbfdc-190">This sample applies this convention to routes that are not using attribute routing where the controller has  "Namespace" in its name.</span></span> <span data-ttu-id="bbfdc-191">Następujący kontroler demonstruje tę Konwencję:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-191">The following controller demonstrates this convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a><span data-ttu-id="bbfdc-192">Użycie modelu aplikacji w WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="bbfdc-192">Application Model Usage in WebApiCompatShim</span></span>

<span data-ttu-id="bbfdc-193">ASP.NET Core MVC używa różnych zestawów Konwencji z ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-193">ASP.NET Core MVC uses a different set of conventions from ASP.NET Web API 2.</span></span> <span data-ttu-id="bbfdc-194">Przy użyciu konwencji niestandardowych można zmodyfikować zachowanie aplikacji ASP.NET Core MVC, aby była spójna z tą aplikacją interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-194">Using custom conventions, you can modify an ASP.NET Core MVC app's behavior to be consistent with that of a Web API app.</span></span> <span data-ttu-id="bbfdc-195">Firma Microsoft dostarcza do tego celu [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) .</span><span class="sxs-lookup"><span data-stu-id="bbfdc-195">Microsoft ships the [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specifically for this purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="bbfdc-196">Dowiedz się więcej o [migracji z interfejsu API sieci Web ASP.NET](xref:migration/webapi).</span><span class="sxs-lookup"><span data-stu-id="bbfdc-196">Learn more about [migration from ASP.NET Web API](xref:migration/webapi).</span></span>

<span data-ttu-id="bbfdc-197">Aby użyć podkładki zgodności z interfejsem API sieci Web, należy dodać pakiet do projektu, a następnie dodać konwencje do MVC, wywołując `AddWebApiConventions` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-197">To use the Web API Compatibility Shim, you need to add the package to your project and then add the conventions to MVC by calling `AddWebApiConventions` in `Startup`:</span></span>

```csharp
services.AddMvc().AddWebApiConventions();
```

<span data-ttu-id="bbfdc-198">Konwencje dostarczone przez podkładkę są stosowane tylko do części aplikacji, do których zastosowano określone atrybuty.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-198">The conventions provided by the shim are only applied to parts of the app that have had certain attributes applied to them.</span></span> <span data-ttu-id="bbfdc-199">Następujące cztery atrybuty są używane do kontrolowania kontrolerów, które powinny być modyfikowane przez konwencje podkładki:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-199">The following four attributes are used to control which controllers should have their conventions modified by the shim's conventions:</span></span>

* [<span data-ttu-id="bbfdc-200">UseWebApiActionConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="bbfdc-200">UseWebApiActionConventionsAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [<span data-ttu-id="bbfdc-201">UseWebApiOverloadingAttribute</span><span class="sxs-lookup"><span data-stu-id="bbfdc-201">UseWebApiOverloadingAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [<span data-ttu-id="bbfdc-202">UseWebApiParameterConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="bbfdc-202">UseWebApiParameterConventionsAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [<span data-ttu-id="bbfdc-203">UseWebApiRoutesAttribute</span><span class="sxs-lookup"><span data-stu-id="bbfdc-203">UseWebApiRoutesAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a><span data-ttu-id="bbfdc-204">Konwencje akcji</span><span class="sxs-lookup"><span data-stu-id="bbfdc-204">Action Conventions</span></span>

<span data-ttu-id="bbfdc-205">`UseWebApiActionConventionsAttribute` jest używany do mapowania metody HTTP na akcje na podstawie ich nazwy (na przykład, `Get` być mapowane na `HttpGet`).</span><span class="sxs-lookup"><span data-stu-id="bbfdc-205">The `UseWebApiActionConventionsAttribute` is used to map the HTTP method to actions based on their name (for instance, `Get` would map to `HttpGet`).</span></span> <span data-ttu-id="bbfdc-206">Ma zastosowanie tylko do akcji, które nie używają routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-206">It only applies to actions that don't use attribute routing.</span></span>

### <a name="overloading"></a><span data-ttu-id="bbfdc-207">Przeciążenie</span><span class="sxs-lookup"><span data-stu-id="bbfdc-207">Overloading</span></span>

<span data-ttu-id="bbfdc-208">`UseWebApiOverloadingAttribute` jest używany do zastosowania konwencji `WebApiOverloadingApplicationModelConvention`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-208">The `UseWebApiOverloadingAttribute` is used to apply the `WebApiOverloadingApplicationModelConvention` convention.</span></span> <span data-ttu-id="bbfdc-209">Ta konwencja dodaje `OverloadActionConstraint` do procesu wyboru akcji, który ogranicza akcje kandydatów do tych, dla których żądanie spełnia wszystkie parametry Nieopcjonalne.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-209">This convention adds an `OverloadActionConstraint` to the action selection process, which limits candidate actions to those for which the request satisfies all non-optional parameters.</span></span>

### <a name="parameter-conventions"></a><span data-ttu-id="bbfdc-210">Konwencje parametrów</span><span class="sxs-lookup"><span data-stu-id="bbfdc-210">Parameter Conventions</span></span>

<span data-ttu-id="bbfdc-211">`UseWebApiParameterConventionsAttribute` służy do stosowania Konwencji akcji `WebApiParameterConventionsApplicationModelConvention`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-211">The `UseWebApiParameterConventionsAttribute` is used to apply the `WebApiParameterConventionsApplicationModelConvention` action convention.</span></span> <span data-ttu-id="bbfdc-212">Ta Konwencja określa, że proste typy używane jako parametry akcji są powiązane z identyfikatorem URI domyślnie, natomiast typy złożone są powiązane z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-212">This convention specifies that simple types used as action parameters are bound from the URI by default, while complex types are bound from the request body.</span></span>

### <a name="routes"></a><span data-ttu-id="bbfdc-213">Trasy</span><span class="sxs-lookup"><span data-stu-id="bbfdc-213">Routes</span></span>

<span data-ttu-id="bbfdc-214">`UseWebApiRoutesAttribute` kontroluje, czy stosowana jest Konwencja kontrolera `WebApiApplicationModelConvention`.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-214">The `UseWebApiRoutesAttribute` controls whether the `WebApiApplicationModelConvention` controller convention is applied.</span></span> <span data-ttu-id="bbfdc-215">Po włączeniu ta konwencja jest używana do dodawania obsługi [obszarów](xref:mvc/controllers/areas) do trasy.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-215">When enabled, this convention is used to add support for [areas](xref:mvc/controllers/areas) to the route.</span></span>

<span data-ttu-id="bbfdc-216">Oprócz zestawu Konwencji pakiet zgodności zawiera `System.Web.Http.ApiController` klasę bazową, która zastępuje ten, który jest dostarczany przez internetowy interfejs API.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-216">In addition to a set of conventions, the compatibility package includes a `System.Web.Http.ApiController` base class that replaces the one provided by Web API.</span></span> <span data-ttu-id="bbfdc-217">Dzięki temu kontrolery napisane dla interfejsu API sieci Web i dziedziczą z jej `ApiController`, aby działały tak, jak zostały zaprojektowane, podczas uruchamiania na ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-217">This allows your controllers written for Web API and inheriting from its `ApiController` to work as they were designed, while running on ASP.NET Core MVC.</span></span> <span data-ttu-id="bbfdc-218">Wszystkie atrybuty `UseWebApi*` wymienione wcześniej są stosowane do klasy kontrolera podstawowego.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-218">All of the `UseWebApi*` attributes listed earlier are applied to the base controller class.</span></span> <span data-ttu-id="bbfdc-219">`ApiController` uwidacznia właściwości, metody i typy wyników, które są zgodne z tymi znajdującymi się w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-219">The `ApiController` exposes properties, methods, and result types that are compatible with those found in Web API.</span></span>

## <a name="using-apiexplorer-to-document-your-app"></a><span data-ttu-id="bbfdc-220">Dokumentowanie aplikacji przy użyciu ApiExplorer</span><span class="sxs-lookup"><span data-stu-id="bbfdc-220">Using ApiExplorer to Document Your App</span></span>

<span data-ttu-id="bbfdc-221">Model aplikacji uwidacznia Właściwość [`ApiExplorer`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) na każdym poziomie, którego można użyć do przechodzenia przez strukturę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-221">The application model exposes an [`ApiExplorer`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) property at each level that can be used to traverse the app's structure.</span></span> <span data-ttu-id="bbfdc-222">Może to służyć do [generowania stron pomocy dla interfejsów API sieci Web przy użyciu narzędzi, takich jak Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="bbfdc-222">This can be used to [generate help pages for your Web APIs using tools like Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="bbfdc-223">Właściwość `ApiExplorer` uwidacznia Właściwość `IsVisible`, którą można ustawić, aby określić, które części modelu aplikacji mają być uwidocznione.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-223">The `ApiExplorer` property exposes an `IsVisible` property that can be set to specify which parts of your app's model should be exposed.</span></span> <span data-ttu-id="bbfdc-224">To ustawienie można skonfigurować przy użyciu konwencji:</span><span class="sxs-lookup"><span data-stu-id="bbfdc-224">You can configure this setting using a convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

<span data-ttu-id="bbfdc-225">Korzystając z tej metody (i dodatkowych Konwencji, jeśli jest to wymagane), możesz włączyć lub wyłączyć widoczność interfejsu API na dowolnym poziomie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbfdc-225">Using this approach (and additional conventions if required), you can enable or disable API visibility at any level within your app.</span></span> 
