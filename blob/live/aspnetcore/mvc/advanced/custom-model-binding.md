---
title: "Wiązania niestandardowe modelu"
author: ardalis
description: "Dostosowywanie wiązania modelu w programie ASP.NET MVC Core."
keywords: "Platformy ASP.NET Core, tworzenia powiązania modelu, niestandardowego integratora modelu"
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.assetid: ebd98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: f3fc3d624c3b79d49a886dd85ca8b19147631e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="custom-model-binding"></a><span data-ttu-id="1e3d6-104">Wiązania niestandardowe modelu</span><span class="sxs-lookup"><span data-stu-id="1e3d6-104">Custom Model Binding</span></span>

<span data-ttu-id="1e3d6-105">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1e3d6-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1e3d6-106">Wiązanie modelu umożliwia akcji kontrolera pracować bezpośrednio z modelu typów (przekazany jako argumenty metody), a nie niż żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-106">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="1e3d6-107">Mapowanie między przychodzącego żądania danych i aplikacji modele jest obsługiwany przez integratorów modeli.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-107">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="1e3d6-108">Deweloperzy mogą rozszerzać funkcjonalność powiązanie modelu wbudowanych zaimplementowanie integratorów modeli niestandardowe (chociaż zazwyczaj nie trzeba zapisać własnego dostawcę).</span><span class="sxs-lookup"><span data-stu-id="1e3d6-108">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="1e3d6-109">Przykładowy widok lub pobrania z witryny GitHub</span><span class="sxs-lookup"><span data-stu-id="1e3d6-109">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="1e3d6-110">Domyślne ograniczenia integratora modelu</span><span class="sxs-lookup"><span data-stu-id="1e3d6-110">Default model binder limitations</span></span>

<span data-ttu-id="1e3d6-111">Domyślne integratorów modeli obsługuje większość popularnych typów danych .NET Core i powinna spełniać potrzeby większość deweloperów.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-111">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="1e3d6-112">Oczekiwane powiązać tekstowych danych wejściowych z żądania bezpośrednio do typów modeli.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-112">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="1e3d6-113">Konieczne może być Przekształć dane wejściowe przed jej powiązania.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-113">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="1e3d6-114">Na przykład, jeśli masz klucz, który może służyć do wyszukiwania danych modelu.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-114">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="1e3d6-115">Można pobrać danych opartą na kluczu, można użyć niestandardowego integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-115">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="1e3d6-116">Przejrzyj powiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="1e3d6-116">Model binding review</span></span>

<span data-ttu-id="1e3d6-117">Wiązanie modelu używa definicji określonych typów, który działa na.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-117">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="1e3d6-118">A *typu prostego* jest konwertowana z jednego ciągu w danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-118">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="1e3d6-119">A *typu złożonego* jest konwertowana z wielu wartości wejściowe.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-119">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="1e3d6-120">Platformę określa różnicę oparte na istnienie `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-120">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="1e3d6-121">Zalecane jest tworzenie konwertera typów, jeśli masz prostą `string`  ->  `SomeType` mapowania, które nie wymagają zasobów zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-121">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="1e3d6-122">Przed utworzeniem własnego niestandardowego integratora modelu, to warto recenzowania jak istniejący model i integratorów są implementowane.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-122">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="1e3d6-123">Należy wziąć pod uwagę [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) które mogą służyć do konwertowania ciągów algorytmem base64 na tablice typu byte.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-123">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="1e3d6-124">Tablice bajtów często są przechowywane jako pliki lub pola obiektu BLOB bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-124">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="1e3d6-125">Praca z ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="1e3d6-125">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="1e3d6-126">Ciągi algorytmem Base64 może służyć do reprezentowania danych binarnych.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-126">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="1e3d6-127">Na przykład poniższy obraz mogą być kodowane jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-127">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="1e3d6-128">![DotNet bot](custom-model-binding/images/bot.png "dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="1e3d6-128">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="1e3d6-129">Mała część ciągu zakodowanego pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-129">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="1e3d6-130">![zakodowany bot DotNet](custom-model-binding/images/encoded-bot.png "zakodowane bot dotnet")</span><span class="sxs-lookup"><span data-stu-id="1e3d6-130">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="1e3d6-131">Postępuj zgodnie z instrukcjami [README w próbce](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) można przekonwertować ciągu zakodowanego algorytmem base64 do pliku.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-131">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="1e3d6-132">ASP.NET Core MVC może przejąć ciągi znaków algorytmem base64 i użyć `ByteArrayModelBinder` przekonwertować go do tablicy typu byte.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-132">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="1e3d6-133">[ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) z zaimplementowanym [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mapy `byte[]` argumenty `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-133">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="1e3d6-134">Podczas tworzenia własnego niestandardowego integratora modelu, można zaimplementować własne `IModelBinderProvider` wpisz lub użyj [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="1e3d6-134">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="1e3d6-135">Poniższy przykład przedstawia użycie `ByteArrayModelBinder` do przekonwertowania ciągu zakodowanego algorytmem base64 `byte[]` i zapisać wynik w pliku:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-135">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="1e3d6-136">Możesz zamieścić ciąg kodowany w formacie base64 dla tej metody interfejsu api przy użyciu narzędzia, takiego jak [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="1e3d6-136">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="1e3d6-137">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="1e3d6-137">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="1e3d6-138">Tak długo, jak obiekt wiążący można powiązać dane żądania odpowiednio nazwane właściwości lub argumentów, powiedzie się wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-138">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="1e3d6-139">Poniższy przykład przedstawia użycie `ByteArrayModelBinder` z modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-139">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="1e3d6-140">Przykładowe integratora modelu niestandardowych</span><span class="sxs-lookup"><span data-stu-id="1e3d6-140">Custom model binder sample</span></span>

<span data-ttu-id="1e3d6-141">W tej sekcji możemy implementacji niestandardowego integratora modelu który:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-141">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="1e3d6-142">Konwertuje przychodzące żądanie danych jednoznacznie argumenty klucza.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-142">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="1e3d6-143">Używa programu Entity Framework Core, aby pobrać skojarzonej jednostki.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-143">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="1e3d6-144">Przekazuje skojarzonej jednostki jako argument do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-144">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="1e3d6-145">Następujące przykładowe używa `ModelBinder` atrybutu `Author` modelu:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-145">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="1e3d6-146">W powyższym kodzie `ModelBinder` atrybut określa typ `IModelBinder` która powinna być używana do powiązania `Author` parametry akcji.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-146">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="1e3d6-147">`AuthorEntityBinder` Jest używana do powiązania `Author` parametru przez pobieranie jednostkę ze źródła danych przy użyciu programu Entity Framework Core i `authorId`:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-147">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="1e3d6-148">Poniższy kod przedstawia sposób użycia `AuthorEntityBinder` w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-148">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="1e3d6-149">`ModelBinder` Atrybut może służyć do zastosowania `AuthorEntityBinder` parametrów, które nie korzystają z domyślnych Konwencji:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-149">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that do not use default conventions:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="1e3d6-150">W tym przykładzie, ponieważ nazwa argumentu nie jest domyślnie `authorId`, został określony przy użyciu parametru `ModelBinder` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-150">In this example, since the name of the argument is not the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="1e3d6-151">Należy pamiętać, uproszczone metody akcji i kontrolerów w porównaniu do wyszukiwania jednostki w metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-151">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="1e3d6-152">Logika pobrać autor przy użyciu programu Entity Framework Core została przeniesiona do integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-152">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="1e3d6-153">Może to być znacznego uproszczenia, jeśli istnieje kilka metod powiązać modelu autora, które mogą pomóc w wykonaj [suchej zasady](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="1e3d6-153">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="1e3d6-154">Możesz zastosować `ModelBinder` atrybutu poszczególnych właściwości (takie jak na viewmodel) lub parametrami metody akcji, aby określić pewnych integratora modelu lub modelu nazw dla właśnie tego typu lub akcji.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-154">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="1e3d6-155">Implementowanie ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="1e3d6-155">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="1e3d6-156">Zamiast stosowania atrybutu, można zaimplementować `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-156">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="1e3d6-157">Jest to implementowania integratorów wbudowana Struktura.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-157">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="1e3d6-158">Po określeniu typu integratora sieci działa na, określ typ argumentu generuje, **nie** akceptuje integratora Twoje dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-158">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="1e3d6-159">Następujących dostawców integratora współpracuje z `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-159">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="1e3d6-160">Gdy jest ona dodawana do kolekcji dostawców MVC, nie trzeba używać `ModelBinder` atrybutu `Author` lub `Author` wpisane parametrów.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-160">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="1e3d6-161">Uwaga: Zwraca poprzedni kod `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-161">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="1e3d6-162">`BinderTypeModelBinder`działa jako fabryka integratorów modeli i zapewnia iniekcji zależności (Podpisane).</span><span class="sxs-lookup"><span data-stu-id="1e3d6-162">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="1e3d6-163">`AuthorEntityBinder` Wymaga Podpisane EF Core dostępu do.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-163">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="1e3d6-164">Użyj `BinderTypeModelBinder` Jeśli Twoje integratora modelu wymaga usług z Podpisane.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-164">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="1e3d6-165">Aby użyć dostawcę integratora modelu niestandardowe, dodaj ją w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-165">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="1e3d6-166">Podczas obliczania integratorów modeli, Kolekcja dostawców jest badana w kolejności.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-166">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="1e3d6-167">Pierwszy dostawcy, który zwraca obiekt wiążący jest używany.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-167">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="1e3d6-168">Na poniższej ilustracji przedstawiono domyślne integratorów modeli z debugera.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-168">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="1e3d6-169">![Domyślna integratorów modeli](custom-model-binding/images/default-model-binders.png "domyślne integratorów modeli")</span><span class="sxs-lookup"><span data-stu-id="1e3d6-169">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="1e3d6-170">Dodawanie dostawcy do końca kolekcji może spowodować integratora modelu wbudowanych, wywoływana przed użytkownika niestandardowego integratora jest stosowany.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-170">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="1e3d6-171">W tym przykładzie dodaniu dostawcy niestandardowego na początku kolekcji, aby upewnić się, jest on używany do `Author` argumentów akcji.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-171">In this example, the custom provider is added to the beginning of the collection to ensure it is used for `Author` action arguments.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="1e3d6-172">Zalecenia i najlepsze rozwiązania</span><span class="sxs-lookup"><span data-stu-id="1e3d6-172">Recommendations and best practices</span></span>

<span data-ttu-id="1e3d6-173">Integratorów modeli niestandardowych:</span><span class="sxs-lookup"><span data-stu-id="1e3d6-173">Custom model binders:</span></span>
- <span data-ttu-id="1e3d6-174">Nie powinny podejmować próby ustawiania kodów stanu lub zwracania wyników (na przykład 404 — Nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="1e3d6-174">Should not attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="1e3d6-175">W przypadku niepowodzenia wiązania modelu [filtr akcji](xref:mvc/controllers/filters) lub logiki w ramach tej metody akcji powinna obsługiwać awarii.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-175">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="1e3d6-176">Są najbardziej przydatny w przypadku wyeliminowanie powtarzających się kodu oraz problemów powiązanych z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-176">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="1e3d6-177">Zwykle nie należy używać do przekonwertowania ciągu na typ niestandardowy, [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) jest zwykle lepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="1e3d6-177">Typically should not be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
