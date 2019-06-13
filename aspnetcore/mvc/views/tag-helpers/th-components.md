---
title: Składniki Pomocnika tagów w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jakie są składniki pomocnika tagów i sposobu ich używania w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.date: 06/12/2019
uid: mvc/views/tag-helpers/th-components
ms.openlocfilehash: b5b3abea6492cfaa7d6acd0e54073a8db12eb2a5
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034751"
---
# <a name="tag-helper-components-in-aspnet-core"></a><span data-ttu-id="4f654-103">Składniki Pomocnika tagów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f654-103">Tag Helper Components in ASP.NET Core</span></span>

<span data-ttu-id="4f654-104">Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Hasan Fiyaz pojemnika](https://github.com/fiyazbinhasan)</span><span class="sxs-lookup"><span data-stu-id="4f654-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Fiyaz Bin Hasan](https://github.com/fiyazbinhasan)</span></span>

<span data-ttu-id="4f654-105">Składnik pomocnika tagów jest pomocnika tagów, która pozwala na warunkowo zmodyfikować lub dodać elementów HTML w kodzie po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="4f654-105">A Tag Helper Component is a Tag Helper that allows you to conditionally modify or add HTML elements from server-side code.</span></span> <span data-ttu-id="4f654-106">Ta funkcja jest dostępna w programie ASP.NET Core 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="4f654-106">This feature is available in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="4f654-107">Platforma ASP.NET Core obejmuje dwie wbudowane składniki pomocnika tagów: `head` i `body`.</span><span class="sxs-lookup"><span data-stu-id="4f654-107">ASP.NET Core includes two built-in Tag Helper Components: `head` and `body`.</span></span> <span data-ttu-id="4f654-108">Znajdują się one w <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> przestrzeni nazw i mogą być używane w MVC i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="4f654-108">They're located in the <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> namespace and can be used in both MVC and Razor Pages.</span></span> <span data-ttu-id="4f654-109">Składniki Pomocnika tagów nie wymagają rejestracji za pomocą aplikacji w *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4f654-109">Tag Helper Components don't require registration with the app in *_ViewImports.cshtml*.</span></span>

<span data-ttu-id="4f654-110">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4f654-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="use-cases"></a><span data-ttu-id="4f654-111">Przypadki zastosowań</span><span class="sxs-lookup"><span data-stu-id="4f654-111">Use cases</span></span>

<span data-ttu-id="4f654-112">Dwie typowe przypadki użycia składników pomocnika tagów obejmują:</span><span class="sxs-lookup"><span data-stu-id="4f654-112">Two common use cases of Tag Helper Components include:</span></span>

1. [<span data-ttu-id="4f654-113">Wprowadzanie `<link>` do `<head>`.</span><span class="sxs-lookup"><span data-stu-id="4f654-113">Injecting a `<link>` into the `<head>`.</span></span>](#inject-into-html-head-element)
1. [<span data-ttu-id="4f654-114">Wprowadzanie `<script>` do `<body>`.</span><span class="sxs-lookup"><span data-stu-id="4f654-114">Injecting a `<script>` into the `<body>`.</span></span>](#inject-into-html-body-element)

<span data-ttu-id="4f654-115">W poniższych sekcjach opisano te przypadki użycia.</span><span class="sxs-lookup"><span data-stu-id="4f654-115">The following sections describe these use cases.</span></span>

### <a name="inject-into-html-head-element"></a><span data-ttu-id="4f654-116">Wstrzyknięcie do głównego elementu HTML</span><span class="sxs-lookup"><span data-stu-id="4f654-116">Inject into HTML head element</span></span>

<span data-ttu-id="4f654-117">W kodzie HTML `<head>` elementu, pliki CSS są często zaimportowane z kodem HTML `<link>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4f654-117">Inside the HTML `<head>` element, CSS files are commonly imported with the HTML `<link>` element.</span></span> <span data-ttu-id="4f654-118">Poniższy kod wprowadza `<link>` elementu do `<head>` elementu za pomocą `head` składnik pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="4f654-118">The following code injects a `<link>` element into the `<head>` element using the `head` Tag Helper Component:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressStyleTagHelperComponent.cs)]

<span data-ttu-id="4f654-119">W poprzednim kodzie:</span><span class="sxs-lookup"><span data-stu-id="4f654-119">In the preceding code:</span></span>

* <span data-ttu-id="4f654-120">`AddressStyleTagHelperComponent` implementuje <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>.</span><span class="sxs-lookup"><span data-stu-id="4f654-120">`AddressStyleTagHelperComponent` implements <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>.</span></span> <span data-ttu-id="4f654-121">Abstrakcja:</span><span class="sxs-lookup"><span data-stu-id="4f654-121">The abstraction:</span></span>
  * <span data-ttu-id="4f654-122">Umożliwia inicjowanie klasy <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.</span><span class="sxs-lookup"><span data-stu-id="4f654-122">Allows initialization of the class with a <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.</span></span>
  * <span data-ttu-id="4f654-123">Umożliwia korzystanie z składniki pomocnika tagów do dodawania lub modyfikowania elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="4f654-123">Enables the use of Tag Helper Components to add or modify HTML elements.</span></span>
* <span data-ttu-id="4f654-124"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> Właściwość definiuje kolejność renderowania składników.</span><span class="sxs-lookup"><span data-stu-id="4f654-124">The <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> property defines the order in which the Components are rendered.</span></span> <span data-ttu-id="4f654-125">`Order` jest to konieczne w przypadku użycia wielu składników pomocnika tagów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f654-125">`Order` is necessary when there are multiple usages of Tag Helper Components in an app.</span></span>
* <span data-ttu-id="4f654-126"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> porównuje kontekstu wykonania <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> wartość właściwości `head`.</span><span class="sxs-lookup"><span data-stu-id="4f654-126"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> compares the execution context's <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> property value to `head`.</span></span> <span data-ttu-id="4f654-127">Jeśli porównanie zwraca wartość true, zawartość `_style` pola są wstrzykiwane do pliku HTML `<head>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4f654-127">If the comparison evaluates to true, the content of the `_style` field is injected into the HTML `<head>` element.</span></span>

### <a name="inject-into-html-body-element"></a><span data-ttu-id="4f654-128">Wstrzyknięcie do elementu body HTML</span><span class="sxs-lookup"><span data-stu-id="4f654-128">Inject into HTML body element</span></span>

<span data-ttu-id="4f654-129">`body` Składnik pomocnika tagów może wprowadzać `<script>` elementu do `<body>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4f654-129">The `body` Tag Helper Component can inject a `<script>` element into the `<body>` element.</span></span> <span data-ttu-id="4f654-130">Poniższy przykład demonstruje tej techniki:</span><span class="sxs-lookup"><span data-stu-id="4f654-130">The following code demonstrates this technique:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressScriptTagHelperComponent.cs)]

<span data-ttu-id="4f654-131">Oddzielny plik HTML jest używany do przechowywania `<script>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4f654-131">A separate HTML file is used to store the `<script>` element.</span></span> <span data-ttu-id="4f654-132">Plik HTML sprawia, że kod bardziej przejrzystym i będzie łatwiejszy w utrzymaniu.</span><span class="sxs-lookup"><span data-stu-id="4f654-132">The HTML file makes the code cleaner and more maintainable.</span></span> <span data-ttu-id="4f654-133">Powyższy kod odczytuje zawartość *TagHelpers/Templates/AddressToolTipScript.html* i dołącza je z danymi wyjściowymi Pomocnik tagu.</span><span class="sxs-lookup"><span data-stu-id="4f654-133">The preceding code reads the contents of *TagHelpers/Templates/AddressToolTipScript.html* and appends it with the Tag Helper output.</span></span> <span data-ttu-id="4f654-134">*AddressToolTipScript.html* plik zawiera następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="4f654-134">The *AddressToolTipScript.html* file includes the following markup:</span></span>

[!code-html[](th-components/samples/RazorPagesSample/TagHelpers/Templates/AddressToolTipScript.html)]

<span data-ttu-id="4f654-135">Powyższy kod wiąże [Bootstrap etykietka narzędzia elementu widget](https://getbootstrap.com/docs/3.3/javascript/#tooltips) do dowolnego `<address>` element, który zawiera `printable` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="4f654-135">The preceding code binds a [Bootstrap tooltip widget](https://getbootstrap.com/docs/3.3/javascript/#tooltips) to any `<address>` element that includes a `printable` attribute.</span></span> <span data-ttu-id="4f654-136">Efekt jest widoczny, gdy wskaźnik myszy znajduje się nad elementem.</span><span class="sxs-lookup"><span data-stu-id="4f654-136">The effect is visible when a mouse pointer hovers over the element.</span></span>

## <a name="register-a-component"></a><span data-ttu-id="4f654-137">Zarejestruj składnik</span><span class="sxs-lookup"><span data-stu-id="4f654-137">Register a Component</span></span>

<span data-ttu-id="4f654-138">Składnik pomocnika tagów, należy dodać do kolekcji składników pomocnika tagów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f654-138">A Tag Helper Component must be added to the app's Tag Helper Components collection.</span></span> <span data-ttu-id="4f654-139">Istnieją trzy sposoby dodania do kolekcji:</span><span class="sxs-lookup"><span data-stu-id="4f654-139">There are three ways to add to the collection:</span></span>

* [<span data-ttu-id="4f654-140">Składniki Pomocnika tagów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f654-140">Tag Helper Components in ASP.NET Core</span></span>](#tag-helper-components-in-aspnet-core)
  * [<span data-ttu-id="4f654-141">Przypadki użycia</span><span class="sxs-lookup"><span data-stu-id="4f654-141">Use cases</span></span>](#use-cases)
    * [<span data-ttu-id="4f654-142">Wstrzyknięcie do głównego elementu HTML</span><span class="sxs-lookup"><span data-stu-id="4f654-142">Inject into HTML head element</span></span>](#inject-into-html-head-element)
    * [<span data-ttu-id="4f654-143">Wstrzyknięcie do elementu body HTML</span><span class="sxs-lookup"><span data-stu-id="4f654-143">Inject into HTML body element</span></span>](#inject-into-html-body-element)
  * [<span data-ttu-id="4f654-144">Zarejestruj składnik</span><span class="sxs-lookup"><span data-stu-id="4f654-144">Register a Component</span></span>](#register-a-component)
    * [<span data-ttu-id="4f654-145">Rejestracja za pośrednictwem usługi kontenera</span><span class="sxs-lookup"><span data-stu-id="4f654-145">Registration via services container</span></span>](#registration-via-services-container)
    * [<span data-ttu-id="4f654-146">Rejestracja za pośrednictwem pliku Razor</span><span class="sxs-lookup"><span data-stu-id="4f654-146">Registration via Razor file</span></span>](#registration-via-razor-file)
    * [<span data-ttu-id="4f654-147">Rejestracja za pośrednictwem modelu strony lub kontrolera</span><span class="sxs-lookup"><span data-stu-id="4f654-147">Registration via Page Model or controller</span></span>](#registration-via-page-model-or-controller)
  * [<span data-ttu-id="4f654-148">Tworzenie składnika</span><span class="sxs-lookup"><span data-stu-id="4f654-148">Create a Component</span></span>](#create-a-component)
  * [<span data-ttu-id="4f654-149">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4f654-149">Additional resources</span></span>](#additional-resources)

### <a name="registration-via-services-container"></a><span data-ttu-id="4f654-150">Rejestracja za pośrednictwem usługi kontenera</span><span class="sxs-lookup"><span data-stu-id="4f654-150">Registration via services container</span></span>

<span data-ttu-id="4f654-151">Jeśli klasa składnika pomocnika tagów nie jest zarządzane przy użyciu <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>, musi być zarejestrowana w [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) systemu.</span><span class="sxs-lookup"><span data-stu-id="4f654-151">If the Tag Helper Component class isn't managed with <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>, it must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) system.</span></span> <span data-ttu-id="4f654-152">Następujące `Startup.ConfigureServices` kodu rejestrów `AddressStyleTagHelperComponent` i `AddressScriptTagHelperComponent` klasy za pomocą [przejściowych okres istnienia](xref:fundamentals/dependency-injection#lifetime-and-registration-options):</span><span class="sxs-lookup"><span data-stu-id="4f654-152">The following `Startup.ConfigureServices` code registers the `AddressStyleTagHelperComponent` and `AddressScriptTagHelperComponent` classes with a [transient lifetime](xref:fundamentals/dependency-injection#lifetime-and-registration-options):</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/Startup.cs?name=snippet_ConfigureServices&highlight=12-15)]

### <a name="registration-via-razor-file"></a><span data-ttu-id="4f654-153">Rejestracja za pośrednictwem pliku Razor</span><span class="sxs-lookup"><span data-stu-id="4f654-153">Registration via Razor file</span></span>

<span data-ttu-id="4f654-154">Jeśli DI nie jest zarejestrowany składnik pomocnika tagów, może być zarejestrowany ze stronami Razor strony lub widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="4f654-154">If the Tag Helper Component isn't registered with DI, it can be registered from a Razor Pages page or an MVC view.</span></span> <span data-ttu-id="4f654-155">Ta technika jest używane do kontrolowania wprowadzonego kodu znaczników i kolejność wykonywania składnika z pliku Razor.</span><span class="sxs-lookup"><span data-stu-id="4f654-155">This technique is used for controlling the injected markup and the component execution order from a Razor file.</span></span>

<span data-ttu-id="4f654-156">`ITagHelperComponentManager` Służy do dodawania składników pomocnika tagów lub usuń je z poziomu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f654-156">`ITagHelperComponentManager` is used to add Tag Helper Components or remove them from the app.</span></span> <span data-ttu-id="4f654-157">Poniższy przykład demonstruje ta technika, za pomocą `AddressTagHelperComponent`:</span><span class="sxs-lookup"><span data-stu-id="4f654-157">The following code demonstrates this technique with `AddressTagHelperComponent`:</span></span>

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_ITagHelperComponentManager)]

<span data-ttu-id="4f654-158">W poprzednim kodzie:</span><span class="sxs-lookup"><span data-stu-id="4f654-158">In the preceding code:</span></span>

* <span data-ttu-id="4f654-159">`@inject` Wystąpienie zapewnia dyrektywa `ITagHelperComponentManager`.</span><span class="sxs-lookup"><span data-stu-id="4f654-159">The `@inject` directive provides an instance of `ITagHelperComponentManager`.</span></span> <span data-ttu-id="4f654-160">Wystąpienie jest przypisany do zmiennej o nazwie `manager` dostępu podrzędnych w pliku Razor.</span><span class="sxs-lookup"><span data-stu-id="4f654-160">The instance is assigned to a variable named `manager` for access downstream in the Razor file.</span></span>
* <span data-ttu-id="4f654-161">Wystąpienie `AddressTagHelperComponent` zostanie dodany do kolekcji składników pomocnika tagów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f654-161">An instance of `AddressTagHelperComponent` is added to the app's Tag Helper Components collection.</span></span>

<span data-ttu-id="4f654-162">`AddressTagHelperComponent` zostanie zmodyfikowany w celu uwzględnienia Konstruktor, który akceptuje `markup` i `order` parametry:</span><span class="sxs-lookup"><span data-stu-id="4f654-162">`AddressTagHelperComponent` is modified to accommodate a constructor that accepts the `markup` and `order` parameters:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_Constructor)]

<span data-ttu-id="4f654-163">Podany `markup` parametr jest używany w `ProcessAsync` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4f654-163">The provided `markup` parameter is used in `ProcessAsync` as follows:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_ProcessAsync&highlight=10-11)]

### <a name="registration-via-page-model-or-controller"></a><span data-ttu-id="4f654-164">Rejestracja za pośrednictwem modelu strony lub kontrolera</span><span class="sxs-lookup"><span data-stu-id="4f654-164">Registration via Page Model or controller</span></span>

<span data-ttu-id="4f654-165">Jeśli DI nie jest zarejestrowany składnik pomocnika tagów, może być zarejestrowany z modelem strony stron Razor lub kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="4f654-165">If the Tag Helper Component isn't registered with DI, it can be registered from a Razor Pages page model or an MVC controller.</span></span> <span data-ttu-id="4f654-166">Ta technika jest przydatna do oddzielania logiki języka C# w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="4f654-166">This technique is useful for separating C# logic from Razor files.</span></span>

<span data-ttu-id="4f654-167">Umożliwia dostęp do wystąpienia iniekcji konstruktora `ITagHelperComponentManager`.</span><span class="sxs-lookup"><span data-stu-id="4f654-167">Constructor injection is used to access an instance of `ITagHelperComponentManager`.</span></span> <span data-ttu-id="4f654-168">Składnik pomocnika tagów jest dodawany do kolekcji składników pomocnika tagów wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="4f654-168">The Tag Helper Component is added to the instance's Tag Helper Components collection.</span></span> <span data-ttu-id="4f654-169">Następujące modelu strony stron Razor pokazano tej techniki, za pomocą `AddressTagHelperComponent`:</span><span class="sxs-lookup"><span data-stu-id="4f654-169">The following Razor Pages page model demonstrates this technique with `AddressTagHelperComponent`:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/Pages/Index.cshtml.cs?name=snippet_IndexModelClass)]

<span data-ttu-id="4f654-170">W poprzednim kodzie:</span><span class="sxs-lookup"><span data-stu-id="4f654-170">In the preceding code:</span></span>

* <span data-ttu-id="4f654-171">Umożliwia dostęp do wystąpienia iniekcji konstruktora `ITagHelperComponentManager`.</span><span class="sxs-lookup"><span data-stu-id="4f654-171">Constructor injection is used to access an instance of `ITagHelperComponentManager`.</span></span>
* <span data-ttu-id="4f654-172">Wystąpienie `AddressTagHelperComponent` zostanie dodany do kolekcji składników pomocnika tagów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f654-172">An instance of `AddressTagHelperComponent` is added to the app's Tag Helper Components collection.</span></span>

## <a name="create-a-component"></a><span data-ttu-id="4f654-173">Tworzenie składnika</span><span class="sxs-lookup"><span data-stu-id="4f654-173">Create a Component</span></span>

<span data-ttu-id="4f654-174">Aby utworzyć niestandardowy składnik pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="4f654-174">To create a custom Tag Helper Component:</span></span>

* <span data-ttu-id="4f654-175">Utwórz klasę publiczną pochodząca od <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.</span><span class="sxs-lookup"><span data-stu-id="4f654-175">Create a public class deriving from <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.</span></span>
* <span data-ttu-id="4f654-176">Zastosuj [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) do klasy atrybutu.</span><span class="sxs-lookup"><span data-stu-id="4f654-176">Apply an [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) attribute to the class.</span></span> <span data-ttu-id="4f654-177">Określ nazwę docelowego elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="4f654-177">Specify the name of the target HTML element.</span></span>
* <span data-ttu-id="4f654-178">*Opcjonalnie*: Zastosuj [[EditorBrowsable(EditorBrowsableState.Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) atrybutów do klasy, aby wyłączyć wyświetlanie typu w technologii IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="4f654-178">*Optional*: Apply an [[EditorBrowsable(EditorBrowsableState.Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) attribute to the class to suppress the type's display in IntelliSense.</span></span>

<span data-ttu-id="4f654-179">Poniższy kod tworzy niestandardowe składnik pomocnika tagów przeznaczonego `<address>` elementu HTML:</span><span class="sxs-lookup"><span data-stu-id="4f654-179">The following code creates a custom Tag Helper Component that targets the `<address>` HTML element:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponentTagHelper.cs)]

<span data-ttu-id="4f654-180">Użyj niestandardowego `address` składnik pomocnika tagów można wstrzyknąć kod znaczników HTML w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4f654-180">Use the custom `address` Tag Helper Component to inject HTML markup as follows:</span></span>

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

<span data-ttu-id="4f654-181">Poprzedni `ProcessAsync` metoda wprowadza HTML udostępniane <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> do dopasowywania `<address>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4f654-181">The preceding `ProcessAsync` method injects the HTML provided to <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> into the matching `<address>` element.</span></span> <span data-ttu-id="4f654-182">Iniekcja występuje, gdy:</span><span class="sxs-lookup"><span data-stu-id="4f654-182">The injection occurs when:</span></span>

* <span data-ttu-id="4f654-183">Kontekst wykonywania `TagName` wartość właściwości jest równa `address`.</span><span class="sxs-lookup"><span data-stu-id="4f654-183">The execution context's `TagName` property value equals `address`.</span></span>
* <span data-ttu-id="4f654-184">Odpowiedni `<address>` element ma `printable` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="4f654-184">The corresponding `<address>` element has a `printable` attribute.</span></span>

<span data-ttu-id="4f654-185">Na przykład `if` instrukcja zwraca wartość true, podczas przetwarzania następujących `<address>` elementu:</span><span class="sxs-lookup"><span data-stu-id="4f654-185">For example, the `if` statement evaluates to true when processing the following `<address>` element:</span></span>

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_AddressPrintable)]

## <a name="additional-resources"></a><span data-ttu-id="4f654-186">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4f654-186">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
* <xref:mvc/views/tag-helpers/builtin-th/Index>
