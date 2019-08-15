---
title: Oznacz składniki pomocnika w ASP.NET Core
author: scottaddie
description: Informacje o składnikach pomocników tagów i sposobach ich użycia w ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.date: 06/12/2019
uid: mvc/views/tag-helpers/th-components
ms.openlocfilehash: 23e244649350b41e4112d10df63139864e5b4381
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022219"
---
# <a name="tag-helper-components-in-aspnet-core"></a><span data-ttu-id="ebede-103">Oznacz składniki pomocnika w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebede-103">Tag Helper Components in ASP.NET Core</span></span>

<span data-ttu-id="ebede-104">Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Fiyaz bin Hasan](https://github.com/fiyazbinhasan)</span><span class="sxs-lookup"><span data-stu-id="ebede-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Fiyaz Bin Hasan](https://github.com/fiyazbinhasan)</span></span>

<span data-ttu-id="ebede-105">Składnik pomocnika tagów to pomocnik tagów, który umożliwia warunkowe modyfikowanie lub Dodawanie elementów HTML z kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="ebede-105">A Tag Helper Component is a Tag Helper that allows you to conditionally modify or add HTML elements from server-side code.</span></span> <span data-ttu-id="ebede-106">Ta funkcja jest dostępna w ASP.NET Core 2,0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="ebede-106">This feature is available in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="ebede-107">ASP.NET Core obejmuje dwa wbudowane składniki pomocnika tagów: `head` i. `body`</span><span class="sxs-lookup"><span data-stu-id="ebede-107">ASP.NET Core includes two built-in Tag Helper Components: `head` and `body`.</span></span> <span data-ttu-id="ebede-108">Znajdują się one w <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> przestrzeni nazw i mogą być używane zarówno w MVC, jak i Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ebede-108">They're located in the <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> namespace and can be used in both MVC and Razor Pages.</span></span> <span data-ttu-id="ebede-109">Składniki pomocnika tagów nie wymagają rejestracji w aplikacji *_ViewImports. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebede-109">Tag Helper Components don't require registration with the app in *_ViewImports.cshtml*.</span></span>

<span data-ttu-id="ebede-110">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ebede-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="use-cases"></a><span data-ttu-id="ebede-111">Przypadki zastosowań</span><span class="sxs-lookup"><span data-stu-id="ebede-111">Use cases</span></span>

<span data-ttu-id="ebede-112">Dwa typowe przypadki użycia składników pomocnika tagów obejmują:</span><span class="sxs-lookup"><span data-stu-id="ebede-112">Two common use cases of Tag Helper Components include:</span></span>

1. [<span data-ttu-id="ebede-113">`<link>` Wprowadzanie`<head>`do.</span><span class="sxs-lookup"><span data-stu-id="ebede-113">Injecting a `<link>` into the `<head>`.</span></span>](#inject-into-html-head-element)
1. [<span data-ttu-id="ebede-114">`<script>` Wprowadzanie`<body>`do.</span><span class="sxs-lookup"><span data-stu-id="ebede-114">Injecting a `<script>` into the `<body>`.</span></span>](#inject-into-html-body-element)

<span data-ttu-id="ebede-115">W poniższych sekcjach opisano te przypadki użycia.</span><span class="sxs-lookup"><span data-stu-id="ebede-115">The following sections describe these use cases.</span></span>

### <a name="inject-into-html-head-element"></a><span data-ttu-id="ebede-116">Wsuń do elementu głównego HTML</span><span class="sxs-lookup"><span data-stu-id="ebede-116">Inject into HTML head element</span></span>

<span data-ttu-id="ebede-117">Wewnątrz elementu HTML `<head>` pliki CSS są zwykle importowane z elementem HTML. `<link>`</span><span class="sxs-lookup"><span data-stu-id="ebede-117">Inside the HTML `<head>` element, CSS files are commonly imported with the HTML `<link>` element.</span></span> <span data-ttu-id="ebede-118">Poniższy kod `<link>` `head` wprowadza element do elementu przy użyciu składnika pomocnika tagów: `<head>`</span><span class="sxs-lookup"><span data-stu-id="ebede-118">The following code injects a `<link>` element into the `<head>` element using the `head` Tag Helper Component:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressStyleTagHelperComponent.cs)]

<span data-ttu-id="ebede-119">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="ebede-119">In the preceding code:</span></span>

* <span data-ttu-id="ebede-120">`AddressStyleTagHelperComponent`implementuje <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>.</span><span class="sxs-lookup"><span data-stu-id="ebede-120">`AddressStyleTagHelperComponent` implements <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>.</span></span> <span data-ttu-id="ebede-121">Streszczenie:</span><span class="sxs-lookup"><span data-stu-id="ebede-121">The abstraction:</span></span>
  * <span data-ttu-id="ebede-122">Umożliwia inicjowanie klasy z <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.</span><span class="sxs-lookup"><span data-stu-id="ebede-122">Allows initialization of the class with a <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.</span></span>
  * <span data-ttu-id="ebede-123">Umożliwia używanie składników pomocnika tagów do dodawania lub modyfikowania elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="ebede-123">Enables the use of Tag Helper Components to add or modify HTML elements.</span></span>
* <span data-ttu-id="ebede-124"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> Właściwość definiuje kolejność, w jakiej składniki są renderowane.</span><span class="sxs-lookup"><span data-stu-id="ebede-124">The <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> property defines the order in which the Components are rendered.</span></span> <span data-ttu-id="ebede-125">`Order`jest konieczne, gdy w aplikacji istnieje wiele użycia składników pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="ebede-125">`Order` is necessary when there are multiple usages of Tag Helper Components in an app.</span></span>
* <span data-ttu-id="ebede-126"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*>porównuje wartość <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> właściwości kontekstu wykonania z `head`.</span><span class="sxs-lookup"><span data-stu-id="ebede-126"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> compares the execution context's <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> property value to `head`.</span></span> <span data-ttu-id="ebede-127">Jeśli wynikiem porównania jest wartość true, zawartość `_style` pola jest wstrzykiwana do elementu HTML. `<head>`</span><span class="sxs-lookup"><span data-stu-id="ebede-127">If the comparison evaluates to true, the content of the `_style` field is injected into the HTML `<head>` element.</span></span>

### <a name="inject-into-html-body-element"></a><span data-ttu-id="ebede-128">Wsuń do elementu treści HTML</span><span class="sxs-lookup"><span data-stu-id="ebede-128">Inject into HTML body element</span></span>

<span data-ttu-id="ebede-129">Składnik pomocnika `<script>`tagówmoże wstrzyknąć element do `<body>` elementu. `body`</span><span class="sxs-lookup"><span data-stu-id="ebede-129">The `body` Tag Helper Component can inject a `<script>` element into the `<body>` element.</span></span> <span data-ttu-id="ebede-130">Poniższy kod ilustruje tę technikę:</span><span class="sxs-lookup"><span data-stu-id="ebede-130">The following code demonstrates this technique:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressScriptTagHelperComponent.cs)]

<span data-ttu-id="ebede-131">Do przechowywania `<script>` elementu używany jest oddzielny plik HTML.</span><span class="sxs-lookup"><span data-stu-id="ebede-131">A separate HTML file is used to store the `<script>` element.</span></span> <span data-ttu-id="ebede-132">Plik HTML sprawia, że kod jest bardziej przejrzysty i bardziej czytelny.</span><span class="sxs-lookup"><span data-stu-id="ebede-132">The HTML file makes the code cleaner and more maintainable.</span></span> <span data-ttu-id="ebede-133">Poprzedni kod odczytuje zawartość *TagHelpers/templates/AddressToolTipScript.html* i dołącza ją z danymi wyjściowymi pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="ebede-133">The preceding code reads the contents of *TagHelpers/Templates/AddressToolTipScript.html* and appends it with the Tag Helper output.</span></span> <span data-ttu-id="ebede-134">Plik *AddressToolTipScript. html* zawiera następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="ebede-134">The *AddressToolTipScript.html* file includes the following markup:</span></span>

[!code-html[](th-components/samples/RazorPagesSample/TagHelpers/Templates/AddressToolTipScript.html)]

<span data-ttu-id="ebede-135">Poprzedni kod tworzy `<address>` `printable` element [widget etykietki narzędzia Bootstrap](https://getbootstrap.com/docs/3.3/javascript/#tooltips) dla każdego elementu, który zawiera atrybut.</span><span class="sxs-lookup"><span data-stu-id="ebede-135">The preceding code binds a [Bootstrap tooltip widget](https://getbootstrap.com/docs/3.3/javascript/#tooltips) to any `<address>` element that includes a `printable` attribute.</span></span> <span data-ttu-id="ebede-136">Efekt jest widoczny, gdy wskaźnik myszy znajduje się nad elementem.</span><span class="sxs-lookup"><span data-stu-id="ebede-136">The effect is visible when a mouse pointer hovers over the element.</span></span>

## <a name="register-a-component"></a><span data-ttu-id="ebede-137">Rejestrowanie składnika</span><span class="sxs-lookup"><span data-stu-id="ebede-137">Register a Component</span></span>

<span data-ttu-id="ebede-138">Składnik pomocnika tagów należy dodać do kolekcji składników pomocnika tagów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ebede-138">A Tag Helper Component must be added to the app's Tag Helper Components collection.</span></span> <span data-ttu-id="ebede-139">Istnieją trzy sposoby dodawania do kolekcji:</span><span class="sxs-lookup"><span data-stu-id="ebede-139">There are three ways to add to the collection:</span></span>

* [<span data-ttu-id="ebede-140">Rejestracja za pośrednictwem kontenera usług</span><span class="sxs-lookup"><span data-stu-id="ebede-140">Registration via services container</span></span>](#registration-via-services-container)
* [<span data-ttu-id="ebede-141">Rejestracja za pośrednictwem pliku Razor</span><span class="sxs-lookup"><span data-stu-id="ebede-141">Registration via Razor file</span></span>](#registration-via-razor-file)
* [<span data-ttu-id="ebede-142">Rejestracja za pośrednictwem modelu lub kontrolera strony</span><span class="sxs-lookup"><span data-stu-id="ebede-142">Registration via Page Model or controller</span></span>](#registration-via-page-model-or-controller)

### <a name="registration-via-services-container"></a><span data-ttu-id="ebede-143">Rejestracja za pośrednictwem kontenera usług</span><span class="sxs-lookup"><span data-stu-id="ebede-143">Registration via services container</span></span>

<span data-ttu-id="ebede-144">Jeśli Klasa składnika pomocnika tagów nie jest zarządzana za pomocą <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>programu, musi być zarejestrowana w systemie wtrysku [zależności (di)](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="ebede-144">If the Tag Helper Component class isn't managed with <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>, it must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) system.</span></span> <span data-ttu-id="ebede-145">Poniższy `Startup.ConfigureServices` kod `AddressScriptTagHelperComponent` rejestruje klasy i z przejściowym okresem istnienia: [](xref:fundamentals/dependency-injection#lifetime-and-registration-options) `AddressStyleTagHelperComponent`</span><span class="sxs-lookup"><span data-stu-id="ebede-145">The following `Startup.ConfigureServices` code registers the `AddressStyleTagHelperComponent` and `AddressScriptTagHelperComponent` classes with a [transient lifetime](xref:fundamentals/dependency-injection#lifetime-and-registration-options):</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/Startup.cs?name=snippet_ConfigureServices&highlight=12-15)]

### <a name="registration-via-razor-file"></a><span data-ttu-id="ebede-146">Rejestracja za pośrednictwem pliku Razor</span><span class="sxs-lookup"><span data-stu-id="ebede-146">Registration via Razor file</span></span>

<span data-ttu-id="ebede-147">Jeśli składnik pomocnika tagów nie jest zarejestrowany przy użyciu DI, może być zarejestrowany na stronie Razor Pages lub widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="ebede-147">If the Tag Helper Component isn't registered with DI, it can be registered from a Razor Pages page or an MVC view.</span></span> <span data-ttu-id="ebede-148">Ta technika służy do kontrolowania wstrzykniętego znacznika i kolejności wykonywania składników z pliku Razor.</span><span class="sxs-lookup"><span data-stu-id="ebede-148">This technique is used for controlling the injected markup and the component execution order from a Razor file.</span></span>

<span data-ttu-id="ebede-149">`ITagHelperComponentManager`służy do dodawania składników pomocnika tagów lub usuwania ich z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ebede-149">`ITagHelperComponentManager` is used to add Tag Helper Components or remove them from the app.</span></span> <span data-ttu-id="ebede-150">Poniższy kod ilustruje tę technikę `AddressTagHelperComponent`:</span><span class="sxs-lookup"><span data-stu-id="ebede-150">The following code demonstrates this technique with `AddressTagHelperComponent`:</span></span>

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_ITagHelperComponentManager)]

<span data-ttu-id="ebede-151">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="ebede-151">In the preceding code:</span></span>

* <span data-ttu-id="ebede-152">`@inject` Dyrektywa zawiera `ITagHelperComponentManager`wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="ebede-152">The `@inject` directive provides an instance of `ITagHelperComponentManager`.</span></span> <span data-ttu-id="ebede-153">Wystąpienie jest przypisane do zmiennej o nazwie `manager` , aby uzyskać dostęp do niej w pliku Razor.</span><span class="sxs-lookup"><span data-stu-id="ebede-153">The instance is assigned to a variable named `manager` for access downstream in the Razor file.</span></span>
* <span data-ttu-id="ebede-154">Wystąpienie `AddressTagHelperComponent` jest dodawane do kolekcji składników pomocnika tagów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ebede-154">An instance of `AddressTagHelperComponent` is added to the app's Tag Helper Components collection.</span></span>

<span data-ttu-id="ebede-155">`AddressTagHelperComponent`zmodyfikowano, aby pomieścić konstruktora, który akceptuje `markup` parametry `order` i:</span><span class="sxs-lookup"><span data-stu-id="ebede-155">`AddressTagHelperComponent` is modified to accommodate a constructor that accepts the `markup` and `order` parameters:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_Constructor)]

<span data-ttu-id="ebede-156">Podany `markup` parametr jest używany w `ProcessAsync` następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ebede-156">The provided `markup` parameter is used in `ProcessAsync` as follows:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_ProcessAsync&highlight=10-11)]

### <a name="registration-via-page-model-or-controller"></a><span data-ttu-id="ebede-157">Rejestracja za pośrednictwem modelu lub kontrolera strony</span><span class="sxs-lookup"><span data-stu-id="ebede-157">Registration via Page Model or controller</span></span>

<span data-ttu-id="ebede-158">Jeśli składnik pomocnika tagów nie jest zarejestrowany w programie DI, może być zarejestrowany z poziomu modelu strony Razor Pages lub kontrolera MVC.</span><span class="sxs-lookup"><span data-stu-id="ebede-158">If the Tag Helper Component isn't registered with DI, it can be registered from a Razor Pages page model or an MVC controller.</span></span> <span data-ttu-id="ebede-159">Ta technika jest przydatna do oddzielania C# logiki od plików Razor.</span><span class="sxs-lookup"><span data-stu-id="ebede-159">This technique is useful for separating C# logic from Razor files.</span></span>

<span data-ttu-id="ebede-160">Iniekcja konstruktora jest używana w celu uzyskania dostępu `ITagHelperComponentManager`do wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ebede-160">Constructor injection is used to access an instance of `ITagHelperComponentManager`.</span></span> <span data-ttu-id="ebede-161">Składnik pomocnika tagów jest dodawany do kolekcji składników pomocnika tagów wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ebede-161">The Tag Helper Component is added to the instance's Tag Helper Components collection.</span></span> <span data-ttu-id="ebede-162">Następujący Razor Pages model strony ilustruje tę technikę `AddressTagHelperComponent`:</span><span class="sxs-lookup"><span data-stu-id="ebede-162">The following Razor Pages page model demonstrates this technique with `AddressTagHelperComponent`:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/Pages/Index.cshtml.cs?name=snippet_IndexModelClass)]

<span data-ttu-id="ebede-163">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="ebede-163">In the preceding code:</span></span>

* <span data-ttu-id="ebede-164">Iniekcja konstruktora jest używana w celu uzyskania dostępu `ITagHelperComponentManager`do wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ebede-164">Constructor injection is used to access an instance of `ITagHelperComponentManager`.</span></span>
* <span data-ttu-id="ebede-165">Wystąpienie `AddressTagHelperComponent` jest dodawane do kolekcji składników pomocnika tagów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ebede-165">An instance of `AddressTagHelperComponent` is added to the app's Tag Helper Components collection.</span></span>

## <a name="create-a-component"></a><span data-ttu-id="ebede-166">Tworzenie składnika</span><span class="sxs-lookup"><span data-stu-id="ebede-166">Create a Component</span></span>

<span data-ttu-id="ebede-167">Aby utworzyć składnik pomocnika tagów niestandardowych:</span><span class="sxs-lookup"><span data-stu-id="ebede-167">To create a custom Tag Helper Component:</span></span>

* <span data-ttu-id="ebede-168">Utwórz klasę publiczną wyprowadzaną z <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.</span><span class="sxs-lookup"><span data-stu-id="ebede-168">Create a public class deriving from <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.</span></span>
* <span data-ttu-id="ebede-169">Zastosuj atrybut [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) do klasy.</span><span class="sxs-lookup"><span data-stu-id="ebede-169">Apply an [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) attribute to the class.</span></span> <span data-ttu-id="ebede-170">Określ nazwę docelowego elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="ebede-170">Specify the name of the target HTML element.</span></span>
* <span data-ttu-id="ebede-171">*Opcjonalne*: Zastosuj atrybut [[EditorBrowsable (EditorBrowsableState. Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) do klasy, aby pominąć wyświetlanie typu w IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="ebede-171">*Optional*: Apply an [[EditorBrowsable(EditorBrowsableState.Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) attribute to the class to suppress the type's display in IntelliSense.</span></span>

<span data-ttu-id="ebede-172">Poniższy kod tworzy składnik pomocnika tagów niestandardowych, który jest przeznaczony `<address>` dla elementu HTML:</span><span class="sxs-lookup"><span data-stu-id="ebede-172">The following code creates a custom Tag Helper Component that targets the `<address>` HTML element:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponentTagHelper.cs)]

<span data-ttu-id="ebede-173">Użyj składnika pomocnika tagów niestandardowych `address` , aby wstrzyknąć znacznik HTML w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ebede-173">Use the custom `address` Tag Helper Component to inject HTML markup as follows:</span></span>

```csharp
public class AddressTagHelperComponent : TagHelperComponent
{
    private readonly string _printableButton =
        "<button type='button' class='btn btn-info' onclick=\"window.open("
        "'https://binged.it/2AXRRYw')\">" +
        "<span class='glyphicon glyphicon-road' aria-hidden='true'></span>" +
        "</button>";

    public override int Order => 3;

    public override async Task ProcessAsync(TagHelperContext context,
                                            TagHelperOutput output)
    {
        if (string.Equals(context.TagName, "address",
                StringComparison.OrdinalIgnoreCase) &&
            output.Attributes.ContainsName("printable"))
        {
            var content = await output.GetChildContentAsync();
            output.Content.SetHtmlContent(
                $"<div>{content.GetContent()}</div>{_printableButton}");
        }
    }
}
```

<span data-ttu-id="ebede-174">Poprzednia `ProcessAsync` metoda wprowadza kod HTML udostępniony do <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> zgodnego `<address>` elementu.</span><span class="sxs-lookup"><span data-stu-id="ebede-174">The preceding `ProcessAsync` method injects the HTML provided to <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> into the matching `<address>` element.</span></span> <span data-ttu-id="ebede-175">Iniekcja występuje, gdy:</span><span class="sxs-lookup"><span data-stu-id="ebede-175">The injection occurs when:</span></span>

* <span data-ttu-id="ebede-176">Wartość `TagName` właściwości kontekstu wykonania jest równa `address`.</span><span class="sxs-lookup"><span data-stu-id="ebede-176">The execution context's `TagName` property value equals `address`.</span></span>
* <span data-ttu-id="ebede-177">Odpowiadający `<address>` element `printable` ma atrybut.</span><span class="sxs-lookup"><span data-stu-id="ebede-177">The corresponding `<address>` element has a `printable` attribute.</span></span>

<span data-ttu-id="ebede-178">Na przykład, `if` instrukcja daje w wyniku wartość true podczas przetwarzania następującego `<address>` elementu:</span><span class="sxs-lookup"><span data-stu-id="ebede-178">For example, the `if` statement evaluates to true when processing the following `<address>` element:</span></span>

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_AddressPrintable)]

## <a name="additional-resources"></a><span data-ttu-id="ebede-179">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ebede-179">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
* <xref:mvc/views/tag-helpers/builtin-th/Index>
