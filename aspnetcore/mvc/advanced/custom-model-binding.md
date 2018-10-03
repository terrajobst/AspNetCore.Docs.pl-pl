---
title: Niestandardowe wiązanie modelu w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak wiązanie modelu umożliwia akcji kontrolera do pracy bezpośrednio z typami modelu w programie ASP.NET Core.
ms.author: riande
ms.date: 04/10/2017
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: b8745241b0699d270bb8f3a56ab614b0ca49e64b
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045539"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="01ffd-103">Niestandardowe wiązanie modelu w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01ffd-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="01ffd-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="01ffd-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="01ffd-105">Wiązanie modelu umożliwia akcji kontrolera pracować bezpośrednio z modelu typów (przekazanej jako argumenty tej metody), zamiast niż żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="01ffd-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="01ffd-106">Mapowanie między przychodzących modeli danych i aplikacji żądanie jest obsługiwane przez integratorów modelu.</span><span class="sxs-lookup"><span data-stu-id="01ffd-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="01ffd-107">Deweloperzy mogą rozszerzać funkcję wiązania modelu wbudowanych, implementując integratorów modelu niestandardowego (choć zazwyczaj nie trzeba zapisać własnego dostawcę).</span><span class="sxs-lookup"><span data-stu-id="01ffd-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="01ffd-108">Wyświetlanie lub pobieranie przykładowy z serwisu GitHub</span><span class="sxs-lookup"><span data-stu-id="01ffd-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="01ffd-109">Domyślne ograniczenia integratora modelu</span><span class="sxs-lookup"><span data-stu-id="01ffd-109">Default model binder limitations</span></span>

<span data-ttu-id="01ffd-110">Domyślne integratorów modelu obsługi najbardziej popularnych typów danych platformy .NET Core i powinny spełniać wymagania większości deweloperów.</span><span class="sxs-lookup"><span data-stu-id="01ffd-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="01ffd-111">Oczekuje, że powiązanie tekstowych danych wejściowych z żądania bezpośrednio do typów modelu.</span><span class="sxs-lookup"><span data-stu-id="01ffd-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="01ffd-112">Może być konieczne do przekształcania danych wejściowych przed powiązanie.</span><span class="sxs-lookup"><span data-stu-id="01ffd-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="01ffd-113">Na przykład, jeśli masz klucz, który może służyć do wyszukiwania danych modelu.</span><span class="sxs-lookup"><span data-stu-id="01ffd-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="01ffd-114">Można pobrać danych na podstawie klucza, można użyć niestandardowego integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="01ffd-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="01ffd-115">Przegląd wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="01ffd-115">Model binding review</span></span>

<span data-ttu-id="01ffd-116">Wiązanie modelu używa określonej definicji dla typów, które działa na.</span><span class="sxs-lookup"><span data-stu-id="01ffd-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="01ffd-117">A *typu prostego* jest konwertowany z jednego ciągu w danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="01ffd-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="01ffd-118">A *typu złożonego* jest konwertowana z wielu wartości wejściowych.</span><span class="sxs-lookup"><span data-stu-id="01ffd-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="01ffd-119">Struktura określa różnicę oparte na istnienie `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="01ffd-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="01ffd-120">Zalecane jest tworzenie konwertera typów, jeśli masz prostą `string`  ->  `SomeType` mapowania, która nie wymaga zasobów zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="01ffd-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="01ffd-121">Przed utworzeniem własnego niestandardowego integratora modelu, to warto recenzowania jak istniejący model integratorów są implementowane.</span><span class="sxs-lookup"><span data-stu-id="01ffd-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="01ffd-122">Należy wziąć pod uwagę [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) której można przekonwertować algorytmem Base64 ciągów na tablice typu byte.</span><span class="sxs-lookup"><span data-stu-id="01ffd-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="01ffd-123">Tablice bajtów często są przechowywane jako pliki lub pól obiektu BLOB bazy danych.</span><span class="sxs-lookup"><span data-stu-id="01ffd-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="01ffd-124">Praca z ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="01ffd-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="01ffd-125">Ciągi zakodowane w formacie Base64 może służyć do reprezentowania danych binarnych.</span><span class="sxs-lookup"><span data-stu-id="01ffd-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="01ffd-126">Na przykład poniższa ilustracja mogą być zakodowane jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="01ffd-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="01ffd-127">![DotNet bot](custom-model-binding/images/bot.png "dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="01ffd-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="01ffd-128">Niewielki fragment ciągu zakodowanego pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="01ffd-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="01ffd-129">![bot DotNet zakodowane](custom-model-binding/images/encoded-bot.png "bot dotnet zakodowany")</span><span class="sxs-lookup"><span data-stu-id="01ffd-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="01ffd-130">Postępuj zgodnie z instrukcjami w [README w próbce](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) można przekonwertować ciągu zakodowanego algorytmem base64.</span><span class="sxs-lookup"><span data-stu-id="01ffd-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="01ffd-131">ASP.NET Core MVC wykonać ciąg kodowany w formacie base64 i użyj `ByteArrayModelBinder` Aby przekonwertować go na tablicę bajtów.</span><span class="sxs-lookup"><span data-stu-id="01ffd-131">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="01ffd-132">[ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) który implementuje [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mapuje `byte[]` argumenty `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="01ffd-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

<span data-ttu-id="01ffd-133">Podczas tworzenia własnego niestandardowego integratora modelu, można zaimplementować własną `IModelBinderProvider` wpisz lub użyj [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="01ffd-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="01ffd-134">Poniższy przykład pokazuje, jak używać `ByteArrayModelBinder` do przekonwertowania ciągu zakodowanego algorytmem base64 `byte[]` i zapisać wynik w pliku:</span><span class="sxs-lookup"><span data-stu-id="01ffd-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="01ffd-135">Możesz ZADAĆ ciągu zakodowanego algorytmem base64, aby ta metoda interfejsu api przy użyciu narzędzia, takiego jak [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="01ffd-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="01ffd-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="01ffd-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="01ffd-137">Tak długo, jak obiekt wiążący można powiązać dane żądania poziomu odpowiednio nazwanych właściwości lub argumentów, wiązanie modelu zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="01ffd-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="01ffd-138">Poniższy przykład pokazuje, jak używać `ByteArrayModelBinder` za pomocą modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="01ffd-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="01ffd-139">Przykładowy obiekt wiążący modelu niestandardowego</span><span class="sxs-lookup"><span data-stu-id="01ffd-139">Custom model binder sample</span></span>

<span data-ttu-id="01ffd-140">W tej sekcji firma Microsoft zaimplementowania niestandardowego integratora modelu który:</span><span class="sxs-lookup"><span data-stu-id="01ffd-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="01ffd-141">Konwertuje dane żądania przychodzące na silnie typizowaną kluczowych argumentów.</span><span class="sxs-lookup"><span data-stu-id="01ffd-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="01ffd-142">Używa platformy Entity Framework Core, aby pobrać skojarzone jednostki.</span><span class="sxs-lookup"><span data-stu-id="01ffd-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="01ffd-143">Przekazuje skojarzona jednostka jako argument do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="01ffd-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="01ffd-144">Następujące przykładowe używa `ModelBinder` atrybutu na `Author` modelu:</span><span class="sxs-lookup"><span data-stu-id="01ffd-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="01ffd-145">W poprzednim kodzie `ModelBinder` atrybut określa typ `IModelBinder` która powinna być używana do powiązania `Author` parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="01ffd-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="01ffd-146">`AuthorEntityBinder` Do powiązania zostanie użyte `Author` parametru przez pobieranie jednostki ze źródła danych przy użyciu platformy Entity Framework Core i `authorId`:</span><span class="sxs-lookup"><span data-stu-id="01ffd-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="01ffd-147">Poniższy kod przedstawia sposób użycia `AuthorEntityBinder` w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="01ffd-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="01ffd-148">`ModelBinder` Atrybut może służyć do zastosowania `AuthorEntityBinder` do parametrów, które nie używają domyślnych Konwencji:</span><span class="sxs-lookup"><span data-stu-id="01ffd-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="01ffd-149">W tym przykładzie, ponieważ nazwa argumentu nie jest domyślnie `authorId`, jest określony przy użyciu parametru `ModelBinder` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="01ffd-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="01ffd-150">Należy pamiętać, uproszczone metody akcji i kontrolerów w porównaniu do wyszukiwania jednostki w metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="01ffd-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="01ffd-151">Logic Apps można pobrać autor przy użyciu platformy Entity Framework Core została przeniesiona do integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="01ffd-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="01ffd-152">Może to być znaczne uproszczenie, jeśli masz kilka metod, które należy powiązać `Author` modelu i może ułatwić Ci wykonać [susz zasady](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="01ffd-152">This can be a considerable simplification when you have several methods that bind to the `Author` model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="01ffd-153">Można zastosować `ModelBinder` atrybutu do właściwości określonego modelu (takich jak na viewmodel) lub parametrami metody akcji, aby określić niektórych integratora modelu lub modelu po prostu tego typu lub akcji.</span><span class="sxs-lookup"><span data-stu-id="01ffd-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="01ffd-154">Implementowanie ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="01ffd-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="01ffd-155">Zamiast stosowania atrybutu, można zaimplementować `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="01ffd-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="01ffd-156">Jest to, jak integratorów wbudowana struktura są implementowane.</span><span class="sxs-lookup"><span data-stu-id="01ffd-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="01ffd-157">Po określeniu typu usługi integratora operuje na, określ typ argumentu daje, **nie** akceptuje swoje integratora danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="01ffd-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="01ffd-158">Dla następującego dostawcy integratorów modeli w programach `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="01ffd-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="01ffd-159">Gdy jest ona dodawana do kolekcji dostawców MVC, nie trzeba używać `ModelBinder` atrybutu na `Author` lub `Author` typizowane parametry.</span><span class="sxs-lookup"><span data-stu-id="01ffd-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="01ffd-160">Uwaga: Zwraca poprzedni kod `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="01ffd-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="01ffd-161">`BinderTypeModelBinder` działa jako fabryki integratorów modelu i zapewnia wstrzykiwanie zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="01ffd-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="01ffd-162">`AuthorEntityBinder` Wymaga DI na dostęp do programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="01ffd-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="01ffd-163">Użyj `BinderTypeModelBinder` Jeśli Twoje integratora modelu wymaga usług od DI.</span><span class="sxs-lookup"><span data-stu-id="01ffd-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="01ffd-164">Aby użyć dostawcę integratora modelu niestandardowego, dodaj ją w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="01ffd-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="01ffd-165">Podczas obliczania integratorów modeli, Kolekcja dostawców jest weryfikowane w kolejności.</span><span class="sxs-lookup"><span data-stu-id="01ffd-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="01ffd-166">Pierwszy dostawca, która zwraca obiekt wiążący jest używany.</span><span class="sxs-lookup"><span data-stu-id="01ffd-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="01ffd-167">Na poniższej ilustracji przedstawiono domyślne integratorów modelu z debugera.</span><span class="sxs-lookup"><span data-stu-id="01ffd-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="01ffd-168">![domyślne integratorów](custom-model-binding/images/default-model-binders.png "domyślne integratorów modelu")</span><span class="sxs-lookup"><span data-stu-id="01ffd-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="01ffd-169">Dodawanie dostawcy do końca kolekcji może spowodować integratora modelu wbudowanych, wywoływana przed Twojego niestandardowego integratora ma szansę.</span><span class="sxs-lookup"><span data-stu-id="01ffd-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="01ffd-170">W tym przykładzie dostawca niestandardowy zostanie dodany na początku kolekcji, aby upewnić się, jest on używany do `Author` argumentów akcji.</span><span class="sxs-lookup"><span data-stu-id="01ffd-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="01ffd-171">Zalecenia i najlepsze rozwiązania</span><span class="sxs-lookup"><span data-stu-id="01ffd-171">Recommendations and best practices</span></span>

<span data-ttu-id="01ffd-172">Integratorów modelu niestandardowego:</span><span class="sxs-lookup"><span data-stu-id="01ffd-172">Custom model binders:</span></span>
- <span data-ttu-id="01ffd-173">Nie należy próbować zestawu kodów stanu lub zwracania wyników (na przykład 404 Nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="01ffd-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="01ffd-174">W przypadku niepowodzenia wiązania modelu [filtr akcji](xref:mvc/controllers/filters) lub logiki w ramach sama metoda akcji powinna obsługiwać awarii.</span><span class="sxs-lookup"><span data-stu-id="01ffd-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="01ffd-175">Są najbardziej przydatne do eliminacji powtarzających się kod i odciąż przekrojowe zagadnienia z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="01ffd-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="01ffd-176">Zazwyczaj nie powinny służyć do przekonwertowania ciągu na typ niestandardowy, [ `TypeConverter` ](/dotnet/api/system.componentmodel.typeconverter) zazwyczaj jest lepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="01ffd-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
