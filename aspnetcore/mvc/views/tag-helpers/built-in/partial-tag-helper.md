---
title: Pomocnik tagu częściowego w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, ASP.NET Core częściowe Tag pomocnika i rolę każdego z jego atrybuty odtwarzania w renderowania widoku częściowego.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: d56df549d845b1f83ec4a5ec97ce6b44438f725a
ms.sourcegitcommit: d22b3c23c45a076c4f394a70b1c8df2fbcdf656d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55428437"
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="9202e-103">Pomocnik tagu częściowego w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9202e-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="9202e-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="9202e-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="9202e-105">Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="9202e-105">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="9202e-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9202e-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="9202e-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="9202e-107">Overview</span></span>

<span data-ttu-id="9202e-108">Częściowe Pomocnik tagu jest używany do renderowania [widoku częściowego](xref:mvc/views/partial) w aplikacjach stronami Razor i programem MVC.</span><span class="sxs-lookup"><span data-stu-id="9202e-108">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="9202e-109">Należy wziąć pod uwagę że:</span><span class="sxs-lookup"><span data-stu-id="9202e-109">Consider that it:</span></span>

* <span data-ttu-id="9202e-110">Wymaga platformy ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="9202e-110">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="9202e-111">Jest to alternatywa [składni pomocnika kodu HTML](xref:mvc/views/partial#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="9202e-111">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#reference-a-partial-view).</span></span>
* <span data-ttu-id="9202e-112">Renderuje widok częściowy asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="9202e-112">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="9202e-113">Opcje pomocnika kodu HTML do renderowania widoku częściowego obejmują:</span><span class="sxs-lookup"><span data-stu-id="9202e-113">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="9202e-114">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="9202e-114">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="9202e-115">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="9202e-115">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="9202e-116">*Produktu* model jest używany w przykładach w tym dokumencie:</span><span class="sxs-lookup"><span data-stu-id="9202e-116">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="9202e-117">Spis atrybutów Pomocnik tagu częściowego poniżej.</span><span class="sxs-lookup"><span data-stu-id="9202e-117">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="9202e-118">nazwa</span><span class="sxs-lookup"><span data-stu-id="9202e-118">name</span></span>

<span data-ttu-id="9202e-119">`name` Atrybut jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="9202e-119">The `name` attribute is required.</span></span> <span data-ttu-id="9202e-120">Wskazuje nazwę lub ścieżkę widoku częściowego do renderowania.</span><span class="sxs-lookup"><span data-stu-id="9202e-120">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="9202e-121">Gdy została podana nazwa widoku częściowego, [widok odnajdywania](xref:mvc/views/overview#view-discovery) proces jest inicjowany.</span><span class="sxs-lookup"><span data-stu-id="9202e-121">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="9202e-122">Ten proces jest pomijany, gdy została podana jawna ścieżka.</span><span class="sxs-lookup"><span data-stu-id="9202e-122">That process is bypassed when an explicit path is provided.</span></span> <span data-ttu-id="9202e-123">Dla wszystkich dopuszczalne `name` wartości, zobacz [odnajdywania widoku częściowego](xref:mvc/views/partial#partial-view-discovery).</span><span class="sxs-lookup"><span data-stu-id="9202e-123">For all acceptable `name` values, see [Partial view discovery](xref:mvc/views/partial#partial-view-discovery).</span></span>

<span data-ttu-id="9202e-124">Następujące znaczniki jest używana jawna ścieżka, co oznacza, że *_ProductPartial.cshtml* ma zostać załadowane z *Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="9202e-124">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="9202e-125">Za pomocą [dla](#for) atrybut modelu są przekazywane do widoku częściowego do powiązania.</span><span class="sxs-lookup"><span data-stu-id="9202e-125">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="9202e-126">dla</span><span class="sxs-lookup"><span data-stu-id="9202e-126">for</span></span>

<span data-ttu-id="9202e-127">`for` Atrybutu przypisuje [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) ma zostać obliczone dla bieżącego modelu.</span><span class="sxs-lookup"><span data-stu-id="9202e-127">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="9202e-128">A `ModelExpression` wnioskuje `@Model.` składni.</span><span class="sxs-lookup"><span data-stu-id="9202e-128">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="9202e-129">Na przykład `for="Product"` mogą być używane zamiast `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="9202e-129">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="9202e-130">To domyślne zachowanie wnioskowania jest zastępowany przy użyciu `@` symbol do definiowania wbudowane wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="9202e-130">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span>

<span data-ttu-id="9202e-131">Ładuje następujące znaczniki *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9202e-131">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="9202e-132">Widok częściowy jest powiązany z modelu skojarzona strona `Product` właściwości:</span><span class="sxs-lookup"><span data-stu-id="9202e-132">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="9202e-133">model</span><span class="sxs-lookup"><span data-stu-id="9202e-133">model</span></span>

<span data-ttu-id="9202e-134">`model` Atrybut przypisuje wystąpienie modelu do przekazania do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="9202e-134">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="9202e-135">`model` Atrybut nie może być używany z [dla](#for) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9202e-135">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="9202e-136">W niej następujące znaczniki nową `Product` obiektu jest tworzone i przekazywane do `model` atrybut do powiązania:</span><span class="sxs-lookup"><span data-stu-id="9202e-136">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="9202e-137">view-data</span><span class="sxs-lookup"><span data-stu-id="9202e-137">view-data</span></span>

<span data-ttu-id="9202e-138">`view-data` Atrybutu przypisuje [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) do przekazania do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="9202e-138">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="9202e-139">Następujące znaczniki sprawia, że całą kolekcję ViewData jest dostępny dla widoku częściowego:</span><span class="sxs-lookup"><span data-stu-id="9202e-139">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="9202e-140">W poprzednim kodzie `IsNumberReadOnly` wartość klucza jest równa `true` i dodawane do kolekcji ViewData.</span><span class="sxs-lookup"><span data-stu-id="9202e-140">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="9202e-141">W związku z tym `ViewData["IsNumberReadOnly"]` jest dostępne w widoku częściowego następujące:</span><span class="sxs-lookup"><span data-stu-id="9202e-141">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="9202e-142">W tym przykładzie wartość `ViewData["IsNumberReadOnly"]` Określa, czy *numer* pole jest wyświetlane jako tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="9202e-142">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="migrate-from-an-html-helper"></a><span data-ttu-id="9202e-143">Migracja z Pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="9202e-143">Migrate from an HTML Helper</span></span>

<span data-ttu-id="9202e-144">Rozważmy następujący przykład pomocnika kodu HTML, asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="9202e-144">Consider the following asynchronous HTML Helper example.</span></span> <span data-ttu-id="9202e-145">Zbiór produktów postanowiliśmy i wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="9202e-145">A collection of products is iterated and displayed.</span></span> <span data-ttu-id="9202e-146">Na `PartialAsync` pierwszy parametr metody *_ProductPartial.cshtml* załadowaniu widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="9202e-146">Per the `PartialAsync` method's first parameter, the *_ProductPartial.cshtml* partial view is loaded.</span></span> <span data-ttu-id="9202e-147">Wystąpienie `Product` modelu są przekazywane do widoku częściowego do powiązania.</span><span class="sxs-lookup"><span data-stu-id="9202e-147">An instance of the `Product` model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_HtmlHelper&highlight=3)]

<span data-ttu-id="9202e-148">Następujące Pomocnik tagu częściowego uzyskuje takie samo zachowanie renderowania asynchronicznego jako `PartialAsync` pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="9202e-148">The following Partial Tag Helper achieves the same asynchronous rendering behavior as the `PartialAsync` HTML Helper.</span></span> <span data-ttu-id="9202e-149">`model` Przypisano atrybut `Product` wystąpienie modelu do powiązania do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="9202e-149">The `model` attribute is assigned a `Product` model instance for binding to the partial view.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_TagHelper&highlight=3)]

## <a name="additional-resources"></a><span data-ttu-id="9202e-150">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9202e-150">Additional resources</span></span>

* <xref:mvc/views/partial>
* <xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag>
