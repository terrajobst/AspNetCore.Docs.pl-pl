---
title: "Wiązania niestandardowe modelu"
author: ardalis
description: "Dostosowywanie wiązania modelu w programie ASP.NET MVC Core."
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: d8b94f53954c5ab63ccf3aab4eb7a7a7dbea487b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="custom-model-binding"></a><span data-ttu-id="74c4a-103">Wiązania niestandardowe modelu</span><span class="sxs-lookup"><span data-stu-id="74c4a-103">Custom Model Binding</span></span>

<span data-ttu-id="74c4a-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="74c4a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="74c4a-105">Wiązanie modelu umożliwia akcji kontrolera pracować bezpośrednio z modelu typów (przekazany jako argumenty metody), a nie niż żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="74c4a-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="74c4a-106">Mapowanie między przychodzącego żądania danych i aplikacji modele jest obsługiwany przez integratorów modeli.</span><span class="sxs-lookup"><span data-stu-id="74c4a-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="74c4a-107">Deweloperzy mogą rozszerzać funkcjonalność powiązanie modelu wbudowanych zaimplementowanie integratorów modeli niestandardowe (chociaż zazwyczaj nie trzeba zapisać własnego dostawcę).</span><span class="sxs-lookup"><span data-stu-id="74c4a-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="74c4a-108">Przykładowy widok lub pobrania z witryny GitHub</span><span class="sxs-lookup"><span data-stu-id="74c4a-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="74c4a-109">Domyślne ograniczenia integratora modelu</span><span class="sxs-lookup"><span data-stu-id="74c4a-109">Default model binder limitations</span></span>

<span data-ttu-id="74c4a-110">Domyślne integratorów modeli obsługuje większość popularnych typów danych .NET Core i powinna spełniać potrzeby większość deweloperów.</span><span class="sxs-lookup"><span data-stu-id="74c4a-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="74c4a-111">Oczekiwane powiązać tekstowych danych wejściowych z żądania bezpośrednio do typów modeli.</span><span class="sxs-lookup"><span data-stu-id="74c4a-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="74c4a-112">Konieczne może być Przekształć dane wejściowe przed jej powiązania.</span><span class="sxs-lookup"><span data-stu-id="74c4a-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="74c4a-113">Na przykład, jeśli masz klucz, który może służyć do wyszukiwania danych modelu.</span><span class="sxs-lookup"><span data-stu-id="74c4a-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="74c4a-114">Można pobrać danych opartą na kluczu, można użyć niestandardowego integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="74c4a-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="74c4a-115">Przejrzyj powiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="74c4a-115">Model binding review</span></span>

<span data-ttu-id="74c4a-116">Wiązanie modelu używa definicji określonych typów, który działa na.</span><span class="sxs-lookup"><span data-stu-id="74c4a-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="74c4a-117">A *typu prostego* jest konwertowana z jednego ciągu w danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="74c4a-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="74c4a-118">A *typu złożonego* jest konwertowana z wielu wartości wejściowe.</span><span class="sxs-lookup"><span data-stu-id="74c4a-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="74c4a-119">Platformę określa różnicę oparte na istnienie `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="74c4a-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="74c4a-120">Zalecane jest tworzenie konwertera typów, jeśli masz prostą `string`  ->  `SomeType` mapowania, które nie wymagają zasobów zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="74c4a-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="74c4a-121">Przed utworzeniem własnego niestandardowego integratora modelu, to warto recenzowania jak istniejący model i integratorów są implementowane.</span><span class="sxs-lookup"><span data-stu-id="74c4a-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="74c4a-122">Należy wziąć pod uwagę [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) które mogą służyć do konwertowania ciągów algorytmem base64 na tablice typu byte.</span><span class="sxs-lookup"><span data-stu-id="74c4a-122">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="74c4a-123">Tablice bajtów często są przechowywane jako pliki lub pola obiektu BLOB bazy danych.</span><span class="sxs-lookup"><span data-stu-id="74c4a-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="74c4a-124">Praca z ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="74c4a-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="74c4a-125">Ciągi algorytmem Base64 może służyć do reprezentowania danych binarnych.</span><span class="sxs-lookup"><span data-stu-id="74c4a-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="74c4a-126">Na przykład poniższy obraz mogą być kodowane jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="74c4a-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="74c4a-127">![DotNet bot](custom-model-binding/images/bot.png "dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="74c4a-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="74c4a-128">Mała część ciągu zakodowanego pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="74c4a-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="74c4a-129">![zakodowany bot DotNet](custom-model-binding/images/encoded-bot.png "zakodowane bot dotnet")</span><span class="sxs-lookup"><span data-stu-id="74c4a-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="74c4a-130">Postępuj zgodnie z instrukcjami [README w próbce](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) można przekonwertować ciągu zakodowanego algorytmem base64 do pliku.</span><span class="sxs-lookup"><span data-stu-id="74c4a-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="74c4a-131">ASP.NET Core MVC może przejąć ciągi znaków algorytmem base64 i użyć `ByteArrayModelBinder` przekonwertować go do tablicy typu byte.</span><span class="sxs-lookup"><span data-stu-id="74c4a-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="74c4a-132">[ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) z zaimplementowanym [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mapy `byte[]` argumenty `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="74c4a-132">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="74c4a-133">Podczas tworzenia własnego niestandardowego integratora modelu, można zaimplementować własne `IModelBinderProvider` wpisz lub użyj [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="74c4a-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="74c4a-134">Poniższy przykład przedstawia użycie `ByteArrayModelBinder` do przekonwertowania ciągu zakodowanego algorytmem base64 `byte[]` i zapisać wynik w pliku:</span><span class="sxs-lookup"><span data-stu-id="74c4a-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="74c4a-135">Możesz zamieścić ciąg kodowany w formacie base64 dla tej metody interfejsu api przy użyciu narzędzia, takiego jak [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="74c4a-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="74c4a-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="74c4a-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="74c4a-137">Tak długo, jak obiekt wiążący można powiązać dane żądania odpowiednio nazwane właściwości lub argumentów, powiedzie się wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="74c4a-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="74c4a-138">Poniższy przykład przedstawia użycie `ByteArrayModelBinder` z modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="74c4a-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="74c4a-139">Przykładowe integratora modelu niestandardowych</span><span class="sxs-lookup"><span data-stu-id="74c4a-139">Custom model binder sample</span></span>

<span data-ttu-id="74c4a-140">W tej sekcji możemy implementacji niestandardowego integratora modelu który:</span><span class="sxs-lookup"><span data-stu-id="74c4a-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="74c4a-141">Konwertuje przychodzące żądanie danych jednoznacznie argumenty klucza.</span><span class="sxs-lookup"><span data-stu-id="74c4a-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="74c4a-142">Używa programu Entity Framework Core, aby pobrać skojarzonej jednostki.</span><span class="sxs-lookup"><span data-stu-id="74c4a-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="74c4a-143">Przekazuje skojarzonej jednostki jako argument do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="74c4a-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="74c4a-144">Następujące przykładowe używa `ModelBinder` atrybutu `Author` modelu:</span><span class="sxs-lookup"><span data-stu-id="74c4a-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="74c4a-145">W powyższym kodzie `ModelBinder` atrybut określa typ `IModelBinder` która powinna być używana do powiązania `Author` parametry akcji.</span><span class="sxs-lookup"><span data-stu-id="74c4a-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="74c4a-146">`AuthorEntityBinder` Jest używana do powiązania `Author` parametru przez pobieranie jednostkę ze źródła danych przy użyciu programu Entity Framework Core i `authorId`:</span><span class="sxs-lookup"><span data-stu-id="74c4a-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="74c4a-147">Poniższy kod przedstawia sposób użycia `AuthorEntityBinder` w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="74c4a-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="74c4a-148">`ModelBinder` Atrybut może służyć do zastosowania `AuthorEntityBinder` parametrów, które nie korzystają z domyślnych Konwencji:</span><span class="sxs-lookup"><span data-stu-id="74c4a-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that do not use default conventions:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="74c4a-149">W tym przykładzie, ponieważ nazwa argumentu nie jest domyślnie `authorId`, został określony przy użyciu parametru `ModelBinder` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="74c4a-149">In this example, since the name of the argument is not the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="74c4a-150">Należy pamiętać, uproszczone metody akcji i kontrolerów w porównaniu do wyszukiwania jednostki w metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="74c4a-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="74c4a-151">Logika pobrać autor przy użyciu programu Entity Framework Core została przeniesiona do integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="74c4a-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="74c4a-152">Może to być znacznego uproszczenia, jeśli istnieje kilka metod powiązać modelu autora, które mogą pomóc w wykonaj [suchej zasady](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="74c4a-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="74c4a-153">Możesz zastosować `ModelBinder` atrybutu poszczególnych właściwości (takie jak na viewmodel) lub parametrami metody akcji, aby określić pewnych integratora modelu lub modelu nazw dla właśnie tego typu lub akcji.</span><span class="sxs-lookup"><span data-stu-id="74c4a-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="74c4a-154">Implementowanie ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="74c4a-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="74c4a-155">Zamiast stosowania atrybutu, można zaimplementować `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="74c4a-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="74c4a-156">Jest to implementowania integratorów wbudowana Struktura.</span><span class="sxs-lookup"><span data-stu-id="74c4a-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="74c4a-157">Po określeniu typu integratora sieci działa na, określ typ argumentu generuje, **nie** akceptuje integratora Twoje dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="74c4a-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="74c4a-158">Następujących dostawców integratora współpracuje z `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="74c4a-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="74c4a-159">Gdy jest ona dodawana do kolekcji dostawców MVC, nie trzeba używać `ModelBinder` atrybutu `Author` lub `Author` wpisane parametrów.</span><span class="sxs-lookup"><span data-stu-id="74c4a-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="74c4a-160">Uwaga: Zwraca poprzedni kod `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="74c4a-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="74c4a-161">`BinderTypeModelBinder`działa jako fabryka integratorów modeli i zapewnia iniekcji zależności (Podpisane).</span><span class="sxs-lookup"><span data-stu-id="74c4a-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="74c4a-162">`AuthorEntityBinder` Wymaga Podpisane EF Core dostępu do.</span><span class="sxs-lookup"><span data-stu-id="74c4a-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="74c4a-163">Użyj `BinderTypeModelBinder` Jeśli Twoje integratora modelu wymaga usług z Podpisane.</span><span class="sxs-lookup"><span data-stu-id="74c4a-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="74c4a-164">Aby użyć dostawcę integratora modelu niestandardowe, dodaj ją w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="74c4a-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="74c4a-165">Podczas obliczania integratorów modeli, Kolekcja dostawców jest badana w kolejności.</span><span class="sxs-lookup"><span data-stu-id="74c4a-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="74c4a-166">Pierwszy dostawcy, który zwraca obiekt wiążący jest używany.</span><span class="sxs-lookup"><span data-stu-id="74c4a-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="74c4a-167">Na poniższej ilustracji przedstawiono domyślne integratorów modeli z debugera.</span><span class="sxs-lookup"><span data-stu-id="74c4a-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="74c4a-168">![Domyślna integratorów modeli](custom-model-binding/images/default-model-binders.png "domyślne integratorów modeli")</span><span class="sxs-lookup"><span data-stu-id="74c4a-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="74c4a-169">Dodawanie dostawcy do końca kolekcji może spowodować integratora modelu wbudowanych, wywoływana przed użytkownika niestandardowego integratora jest stosowany.</span><span class="sxs-lookup"><span data-stu-id="74c4a-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="74c4a-170">W tym przykładzie dodaniu dostawcy niestandardowego na początku kolekcji, aby upewnić się, jest on używany do `Author` argumentów akcji.</span><span class="sxs-lookup"><span data-stu-id="74c4a-170">In this example, the custom provider is added to the beginning of the collection to ensure it is used for `Author` action arguments.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="74c4a-171">Zalecenia i najlepsze rozwiązania</span><span class="sxs-lookup"><span data-stu-id="74c4a-171">Recommendations and best practices</span></span>

<span data-ttu-id="74c4a-172">Integratorów modeli niestandardowych:</span><span class="sxs-lookup"><span data-stu-id="74c4a-172">Custom model binders:</span></span>
- <span data-ttu-id="74c4a-173">Nie powinny podejmować próby ustawiania kodów stanu lub zwracania wyników (na przykład 404 — Nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="74c4a-173">Should not attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="74c4a-174">W przypadku niepowodzenia wiązania modelu [filtr akcji](xref:mvc/controllers/filters) lub logiki w ramach tej metody akcji powinna obsługiwać awarii.</span><span class="sxs-lookup"><span data-stu-id="74c4a-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="74c4a-175">Są najbardziej przydatny w przypadku wyeliminowanie powtarzających się kodu oraz problemów powiązanych z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="74c4a-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="74c4a-176">Zwykle nie należy używać do przekonwertowania ciągu na typ niestandardowy, [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) jest zwykle lepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="74c4a-176">Typically should not be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
