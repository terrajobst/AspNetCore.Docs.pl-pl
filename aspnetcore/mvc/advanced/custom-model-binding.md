---
title: Niestandardowe powiązanie modelu w ASP.NET Core
author: ardalis
description: Dowiedz się, jak powiązanie modelu umożliwia akcjom kontrolera bezpośrednie działanie z typami modeli w ASP.NET Core.
ms.author: riande
ms.date: 11/13/2018
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 91f42393ffee3249f9167e10eaea7b279a7cb70b
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878412"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="110d9-103">Niestandardowe powiązanie modelu w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="110d9-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="110d9-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="110d9-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="110d9-105">Powiązanie modelu umożliwia działanie kontrolera do pracy bezpośrednio z typami modeli (przekazane jako argumenty metod), a nie żądaniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="110d9-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="110d9-106">Mapowanie między danymi żądań przychodzących a modelami aplikacji jest obsługiwane przez program Binders.</span><span class="sxs-lookup"><span data-stu-id="110d9-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="110d9-107">Deweloperzy mogą rozszerzać wbudowaną funkcję powiązania modelu, implementując niestandardowe powiązania modelu (zazwyczaj nie trzeba pisać własnego dostawcy).</span><span class="sxs-lookup"><span data-stu-id="110d9-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="110d9-108">Wyświetlanie lub pobieranie przykładu z usługi GitHub</span><span class="sxs-lookup"><span data-stu-id="110d9-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="110d9-109">Domyślne ograniczenia spinacza modelu</span><span class="sxs-lookup"><span data-stu-id="110d9-109">Default model binder limitations</span></span>

<span data-ttu-id="110d9-110">Domyślne powiązania modeli obsługują większość wspólnych typów danych .NET Core i powinny spełniać większość potrzeb deweloperów.</span><span class="sxs-lookup"><span data-stu-id="110d9-110">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="110d9-111">Oczekują one powiązania danych tekstowych na podstawie żądania bezpośrednio z typami modeli.</span><span class="sxs-lookup"><span data-stu-id="110d9-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="110d9-112">Może być konieczne przekształcenie danych wejściowych przed ich powiązaniem.</span><span class="sxs-lookup"><span data-stu-id="110d9-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="110d9-113">Na przykład, jeśli masz klucz, którego można użyć do wyszukiwania danych modelu.</span><span class="sxs-lookup"><span data-stu-id="110d9-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="110d9-114">Do pobierania danych opartych na kluczu można użyć spinacza modelu niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="110d9-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="110d9-115">Przegląd powiązań modelu</span><span class="sxs-lookup"><span data-stu-id="110d9-115">Model binding review</span></span>

<span data-ttu-id="110d9-116">Powiązanie modelu używa określonych definicji dla typów, w których działa.</span><span class="sxs-lookup"><span data-stu-id="110d9-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="110d9-117">*Typ prosty* jest konwertowany z pojedynczego ciągu w danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="110d9-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="110d9-118">*Typ złożony* jest konwertowany z wielu wartości wejściowych.</span><span class="sxs-lookup"><span data-stu-id="110d9-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="110d9-119">Struktura określa różnice w zależności od istnienia `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="110d9-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="110d9-120">Zalecamy utworzenie konwertera typów, jeśli istnieje proste `string`  ->  `SomeType` mapowanie, które nie wymaga zasobów zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="110d9-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="110d9-121">Przed utworzeniem własnego spinacza modelu niestandardowego warto przejrzeć sposób implementacji istniejących spinaczy modelu.</span><span class="sxs-lookup"><span data-stu-id="110d9-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="110d9-122">Rozważmy [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) , którego można użyć do przekonwertowania ciągów zakodowanych algorytmem Base64 na tablice bajtowe.</span><span class="sxs-lookup"><span data-stu-id="110d9-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="110d9-123">Tablice bajtowe są często przechowywane jako pliki lub pola obiektów BLOB bazy danych.</span><span class="sxs-lookup"><span data-stu-id="110d9-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="110d9-124">Praca z ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="110d9-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="110d9-125">Ciągi kodowane algorytmem Base64 mogą służyć do reprezentowania danych binarnych.</span><span class="sxs-lookup"><span data-stu-id="110d9-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="110d9-126">Na przykład poniższy obraz może być zakodowany jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="110d9-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="110d9-127">![bot dotnet](custom-model-binding/images/bot.png "bot dotnet")</span><span class="sxs-lookup"><span data-stu-id="110d9-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="110d9-128">Na poniższej ilustracji pokazano małą część zakodowanego ciągu:</span><span class="sxs-lookup"><span data-stu-id="110d9-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="110d9-129">![kodowanie dotnet bot](custom-model-binding/images/encoded-bot.png "kodowanie dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="110d9-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="110d9-130">Postępuj zgodnie z instrukcjami podanymi w pliku [README przykładu](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) , aby przekonwertować ciąg zakodowany w formacie base64 na plik.</span><span class="sxs-lookup"><span data-stu-id="110d9-130">Follow the instructions in the [sample's README](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="110d9-131">ASP.NET Core MVC może przyjmować ciąg zakodowany algorytmem Base64 i używać `ByteArrayModelBinder` go, aby przekonwertować go na tablicę bajtów.</span><span class="sxs-lookup"><span data-stu-id="110d9-131">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="110d9-132">[ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) , który implementuje argumenty mapy `byte[]` [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) do `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="110d9-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="110d9-133">Podczas tworzenia własnego spinacza modelu niestandardowego można zaimplementować własny `IModelBinderProvider` typ lub użyć [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="110d9-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="110d9-134">Poniższy przykład pokazuje, jak użyć `ByteArrayModelBinder` do przekonwertowania ciągu zakodowanego algorytmem Base64 `byte[]` na a i zapisać wynik do pliku:</span><span class="sxs-lookup"><span data-stu-id="110d9-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="110d9-135">Można OPUBLIKOWAĆ ciąg zakodowany algorytmem Base64 w tej metodzie interfejsu API za pomocą narzędzia, takiego jak [Poster](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="110d9-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="110d9-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="110d9-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="110d9-137">Tak długo, jak spinacz może powiązać dane żądania do odpowiednio nazwanych właściwości lub argumentów, powiązanie modelu zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="110d9-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="110d9-138">Poniższy przykład przedstawia sposób użycia `ByteArrayModelBinder` z modelem widoku:</span><span class="sxs-lookup"><span data-stu-id="110d9-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="110d9-139">Przykładowy spinacz modelu niestandardowego</span><span class="sxs-lookup"><span data-stu-id="110d9-139">Custom model binder sample</span></span>

<span data-ttu-id="110d9-140">W tej sekcji zaimplementujemy niestandardowy spinacz modelu, który:</span><span class="sxs-lookup"><span data-stu-id="110d9-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="110d9-141">Konwertuje przychodzące dane żądania na argumenty klucza o jednoznacznie określonym typie.</span><span class="sxs-lookup"><span data-stu-id="110d9-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="110d9-142">Używa Entity Framework Core, aby pobrać skojarzoną jednostkę.</span><span class="sxs-lookup"><span data-stu-id="110d9-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="110d9-143">Przekazuje skojarzoną jednostkę jako argument do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="110d9-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="110d9-144">Poniższy przykład używa `ModelBinder` atrybutu `Author` w modelu:</span><span class="sxs-lookup"><span data-stu-id="110d9-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="110d9-145">W poprzednim kodzie `ModelBinder` atrybut określa `IModelBinder` typ, który powinien być używany do wiązania `Author` parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="110d9-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span>

<span data-ttu-id="110d9-146">Następująca `AuthorEntityBinder` Klasa wiąże `Author` parametr przez pobranie jednostki ze `authorId`źródła danych przy użyciu Entity Framework Core i:</span><span class="sxs-lookup"><span data-stu-id="110d9-146">The following `AuthorEntityBinder` class binds an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> <span data-ttu-id="110d9-147">Poprzednia `AuthorEntityBinder` Klasa jest przeznaczona do zilustrowania niestandardowego spinacza modelu.</span><span class="sxs-lookup"><span data-stu-id="110d9-147">The preceding `AuthorEntityBinder` class is intended to illustrate a custom model binder.</span></span> <span data-ttu-id="110d9-148">Klasa nie jest przeznaczona do zilustrowania najlepszych rozwiązań dotyczących scenariusza wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="110d9-148">The class isn't intended to illustrate best practices for a lookup scenario.</span></span> <span data-ttu-id="110d9-149">W celu wyszukania `authorId` należy powiązać bazę danych i zbadać ją w metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="110d9-149">For lookup, bind the `authorId` and query the database in an action method.</span></span> <span data-ttu-id="110d9-150">To podejście oddziela błędy powiązań modelu z `NotFound` przypadków.</span><span class="sxs-lookup"><span data-stu-id="110d9-150">This approach separates model binding failures from `NotFound` cases.</span></span>

<span data-ttu-id="110d9-151">Poniższy kod pokazuje, `AuthorEntityBinder` jak używać w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="110d9-151">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="110d9-152">Ten `ModelBinder` atrybut może służyć do `AuthorEntityBinder` stosowania parametrów do, które nie używają Konwencji domyślnych:</span><span class="sxs-lookup"><span data-stu-id="110d9-152">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="110d9-153">W tym przykładzie, ponieważ nazwa argumentu nie jest wartością domyślną `authorId`, jest określona w parametrze `ModelBinder` przy użyciu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="110d9-153">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="110d9-154">Zarówno kontroler, jak i Metoda akcji są uproszczone w porównaniu z wyszukiwaniem jednostki w metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="110d9-154">Both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="110d9-155">Logika pobierania autora przy użyciu Entity Framework Core jest przenoszona do spinacza modelu.</span><span class="sxs-lookup"><span data-stu-id="110d9-155">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="110d9-156">Może to być znaczące uproszczenie, gdy istnieje kilka metod, które są powiązane z `Author` modelem.</span><span class="sxs-lookup"><span data-stu-id="110d9-156">This can be a considerable simplification when you have several methods that bind to the `Author` model.</span></span>

<span data-ttu-id="110d9-157">Można zastosować `ModelBinder` atrybut do poszczególnych właściwości modelu (na przykład na ViewModel) lub do parametrów metody akcji, aby określić określony spinacz modelu lub nazwę modelu dla tylko tego typu lub akcji.</span><span class="sxs-lookup"><span data-stu-id="110d9-157">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="110d9-158">Implementowanie elementu ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="110d9-158">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="110d9-159">Zamiast stosować atrybut, można zaimplementować `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="110d9-159">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="110d9-160">Jest to sposób implementacji wbudowanych powiązań struktury.</span><span class="sxs-lookup"><span data-stu-id="110d9-160">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="110d9-161">Po określeniu typu, na którym działa Twój spinacz, należy określić typ argumentu, który produkuje, a **nie** dane wejściowe zaakceptowane przez spinacz.</span><span class="sxs-lookup"><span data-stu-id="110d9-161">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="110d9-162">Następujący dostawca programu Binder współpracuje z `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="110d9-162">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="110d9-163">Po dodaniu do kolekcji dostawców MVC nie trzeba używać `ModelBinder` atrybutów z `Author` parametrami lub `Author`typem.</span><span class="sxs-lookup"><span data-stu-id="110d9-163">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author`-typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="110d9-164">Uwaga: Poprzedni kod zwraca `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="110d9-164">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="110d9-165">`BinderTypeModelBinder`działa jako fabryka dla segregatorów modelu i zapewnia iniekcję zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="110d9-165">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="110d9-166">Program `AuthorEntityBinder` wymaga dostępu EF Core.</span><span class="sxs-lookup"><span data-stu-id="110d9-166">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="110d9-167">Użyj `BinderTypeModelBinder` , jeśli model spinacza wymaga usług z di.</span><span class="sxs-lookup"><span data-stu-id="110d9-167">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="110d9-168">Aby użyć niestandardowego dostawcy segregatorów modelu, Dodaj go w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="110d9-168">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="110d9-169">Podczas oceniania powiązań modelu kolekcja dostawców jest sprawdzana w podanej kolejności.</span><span class="sxs-lookup"><span data-stu-id="110d9-169">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="110d9-170">Używany jest pierwszy dostawca zwracający spinacz.</span><span class="sxs-lookup"><span data-stu-id="110d9-170">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="110d9-171">Na poniższej ilustracji przedstawiono domyślne powiązania modelu z debugera.</span><span class="sxs-lookup"><span data-stu-id="110d9-171">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="110d9-172">![domyślne powiązania modelu](custom-model-binding/images/default-model-binders.png "domyślne powiązania modelu")</span><span class="sxs-lookup"><span data-stu-id="110d9-172">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="110d9-173">Dodanie swojego dostawcy do końca kolekcji może spowodować wywołanie wbudowanego spinacza modelu, zanim niestandardowy spinacz będzie miał szansę.</span><span class="sxs-lookup"><span data-stu-id="110d9-173">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="110d9-174">W tym przykładzie niestandardowy dostawca zostanie dodany do początku kolekcji, aby upewnić się, że jest on używany dla `Author` argumentów akcji.</span><span class="sxs-lookup"><span data-stu-id="110d9-174">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

### <a name="polymorphic-model-binding"></a><span data-ttu-id="110d9-175">Powiązanie modelu polimorficznego</span><span class="sxs-lookup"><span data-stu-id="110d9-175">Polymorphic model binding</span></span>

<span data-ttu-id="110d9-176">Powiązanie z różnymi modelami typów pochodnych jest znane jako powiązanie modelu polimorficzne.</span><span class="sxs-lookup"><span data-stu-id="110d9-176">Binding to different models of derived types is known as polymorphic model binding.</span></span> <span data-ttu-id="110d9-177">Niestandardowe powiązanie modelu jest wymagane, gdy wartość żądania musi być powiązana z konkretnym typem modelu pochodnego.</span><span class="sxs-lookup"><span data-stu-id="110d9-177">Custom model binding is required when the request value must be bound to the specific derived model type.</span></span> <span data-ttu-id="110d9-178">Chyba że takie podejście jest wymagane, zalecamy uniknięcie powiązania modelu polimorficznego.</span><span class="sxs-lookup"><span data-stu-id="110d9-178">Unless this approach is required, we recommend avoiding polymorphic model binding.</span></span> <span data-ttu-id="110d9-179">Powiązanie modelu polimorficznego utrudnia powody dotyczące modeli powiązanych.</span><span class="sxs-lookup"><span data-stu-id="110d9-179">Polymorphic model binding makes it difficult to reason about the bound models.</span></span> <span data-ttu-id="110d9-180">Jeśli jednak aplikacja wymaga powiązania modelu polimorficznego, implementacja może wyglądać podobnie do następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="110d9-180">However, if an app requires polymorphic model binding, an implementation might look like the following code:</span></span>

<span data-ttu-id="110d9-181">Powiązanie z różnymi modelami typów pochodnych jest znane jako powiązanie modelu polimorficzne.</span><span class="sxs-lookup"><span data-stu-id="110d9-181">Binding to different models of derived types is known as polymorphic model binding.</span></span> <span data-ttu-id="110d9-182">Niestandardowe powiązanie modelu jest wymagane, gdy wartość żądania musi być powiązana z konkretnym typem modelu pochodnego.</span><span class="sxs-lookup"><span data-stu-id="110d9-182">Custom model binding is required when the request value must be bound to the specific derived model type.</span></span> <span data-ttu-id="110d9-183">Powiązanie modelu polimorficznego:</span><span class="sxs-lookup"><span data-stu-id="110d9-183">Polymorphic model binding:</span></span>

* <span data-ttu-id="110d9-184">Nie jest typowy dla interfejsu API REST, który jest przeznaczony do współpracy ze wszystkimi językami.</span><span class="sxs-lookup"><span data-stu-id="110d9-184">Isn't typical for a REST API that's designed to interoperate with all languages.</span></span>
* <span data-ttu-id="110d9-185">Utrudniają powody dotyczące modeli powiązanych.</span><span class="sxs-lookup"><span data-stu-id="110d9-185">Makes it difficult to reason about the bound models.</span></span>

<span data-ttu-id="110d9-186">Jeśli jednak aplikacja wymaga powiązania modelu polimorficznego, implementacja może wyglądać podobnie do następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="110d9-186">However, if an app requires polymorphic model binding, an implementation might look like the following code:</span></span>

[!code-csharp[](custom-model-binding/3.0sample/PolymorphicModelBinding/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="110d9-187">Zalecenia i najlepsze rozwiązania</span><span class="sxs-lookup"><span data-stu-id="110d9-187">Recommendations and best practices</span></span>

<span data-ttu-id="110d9-188">Niestandardowe powiązania modelu:</span><span class="sxs-lookup"><span data-stu-id="110d9-188">Custom model binders:</span></span>

- <span data-ttu-id="110d9-189">Nie należy próbować ustawiać kodów stanu ani zwracać wyników (na przykład nie znaleziono 404).</span><span class="sxs-lookup"><span data-stu-id="110d9-189">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="110d9-190">Jeśli powiązanie modelu nie powiedzie się, [Filtr akcji](xref:mvc/controllers/filters) lub logika w samej metodzie akcji powinien obsłużyć błąd.</span><span class="sxs-lookup"><span data-stu-id="110d9-190">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="110d9-191">Są najbardziej przydatne do eliminowania powtarzających się kodu i krzyżowego obaw z metod akcji.</span><span class="sxs-lookup"><span data-stu-id="110d9-191">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="110d9-192">Zwykle nie należy używać do konwertowania ciągu na typ niestandardowy, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) jest zazwyczaj lepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="110d9-192">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
