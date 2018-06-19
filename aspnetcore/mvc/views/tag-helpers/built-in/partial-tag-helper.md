---
title: Pomocnik częściowe tagu w platformy ASP.NET Core
author: scottaddie
description: Dostęp do platformy ASP.NET Core częściowe Tag pomocnika i roli każdego z jego atrybuty odtwarzana renderowania widoku częściowego.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 786c333980db89a9a5a60dc70c0bd1998ca159cd
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962597"
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="2478c-103">Pomocnik częściowe tagu w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2478c-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="2478c-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="2478c-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="2478c-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2478c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="2478c-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2478c-106">Overview</span></span>

<span data-ttu-id="2478c-107">Częściowe pomocnika Tag jest używany do renderowania [widoku częściowego](xref:mvc/views/partial) w aplikacjach stron Razor i MVC.</span><span class="sxs-lookup"><span data-stu-id="2478c-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="2478c-108">Należy wziąć pod uwagę że:</span><span class="sxs-lookup"><span data-stu-id="2478c-108">Consider that it:</span></span>

* <span data-ttu-id="2478c-109">Wymaga platformy ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="2478c-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="2478c-110">Jest to alternatywa dla [składni pomocnika kodu HTML](xref:mvc/views/partial#referencing-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="2478c-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>
* <span data-ttu-id="2478c-111">Renderuje widok częściowy asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="2478c-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="2478c-112">Opcje pomocnika kodu HTML do renderowania widoku częściowego obejmują:</span><span class="sxs-lookup"><span data-stu-id="2478c-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="2478c-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="2478c-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="2478c-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="2478c-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="2478c-115">*Produktu* model jest używany w przykłady w tym dokumencie:</span><span class="sxs-lookup"><span data-stu-id="2478c-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="2478c-116">Spis atrybutów częściowe pomocnika tagów jest zgodna.</span><span class="sxs-lookup"><span data-stu-id="2478c-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="2478c-117">nazwa</span><span class="sxs-lookup"><span data-stu-id="2478c-117">name</span></span>

<span data-ttu-id="2478c-118">`name` Atrybut jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="2478c-118">The `name` attribute is required.</span></span> <span data-ttu-id="2478c-119">Wskazuje on, nazwa lub ścieżka widoku częściowego do renderowania.</span><span class="sxs-lookup"><span data-stu-id="2478c-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="2478c-120">Jeśli podano nazwę widoku częściowego, [widok odnajdywania](xref:mvc/views/overview#view-discovery) proces jest inicjowany.</span><span class="sxs-lookup"><span data-stu-id="2478c-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="2478c-121">Ten proces jest pomijane, gdy została podana ścieżka jawna.</span><span class="sxs-lookup"><span data-stu-id="2478c-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="2478c-122">Następujący kod używa jawne ścieżki, co oznacza, że *_ProductPartial.cshtml* ma zostać załadowane z *Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="2478c-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="2478c-123">Przy użyciu [dla](#for) atrybut modelu są przekazywane do widoku częściowego dla powiązania.</span><span class="sxs-lookup"><span data-stu-id="2478c-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="2478c-124">dla</span><span class="sxs-lookup"><span data-stu-id="2478c-124">for</span></span>

<span data-ttu-id="2478c-125">`for` Atrybutu przypisuje [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) ma zostać obliczone dla bieżącego modelu.</span><span class="sxs-lookup"><span data-stu-id="2478c-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="2478c-126">A `ModelExpression` wnioskuje `@Model.` składni.</span><span class="sxs-lookup"><span data-stu-id="2478c-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="2478c-127">Na przykład `for="Product"` można użyć zamiast `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="2478c-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="2478c-128">To domyślne zachowanie wnioskowania zostanie zastąpiona przy użyciu `@` symbolu, aby zdefiniować wyrażenie wbudowanego.</span><span class="sxs-lookup"><span data-stu-id="2478c-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="2478c-129">`for` Nie można użyć atrybutu [modelu](#model) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2478c-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="2478c-130">Następujący kod znaczników ładuje *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2478c-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="2478c-131">Widok częściowy jest powiązana z modelem skojarzonej strony `Product` właściwości:</span><span class="sxs-lookup"><span data-stu-id="2478c-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="2478c-132">model</span><span class="sxs-lookup"><span data-stu-id="2478c-132">model</span></span>

<span data-ttu-id="2478c-133">`model` Atrybut przypisuje wystąpienie modelu do przekazania do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="2478c-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="2478c-134">`model` Nie można użyć atrybutu [dla](#for) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2478c-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="2478c-135">W następujących znaczników nowy `Product` obiektu jest wystąpienie zostało utworzone i przekazane do `model` atrybutu dla powiązania:</span><span class="sxs-lookup"><span data-stu-id="2478c-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="2478c-136">Wyświetlanie danych</span><span class="sxs-lookup"><span data-stu-id="2478c-136">view-data</span></span>

<span data-ttu-id="2478c-137">`view-data` Atrybutu przypisuje [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) do przekazania do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="2478c-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="2478c-138">Następujący kod znaczników sprawia, że całą kolekcję ViewData dostępne do widoku częściowego:</span><span class="sxs-lookup"><span data-stu-id="2478c-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="2478c-139">W powyższym kodzie `IsNumberReadOnly` ma ustawioną wartość klucza `true` i dodać do kolekcji ViewData.</span><span class="sxs-lookup"><span data-stu-id="2478c-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="2478c-140">W rezultacie `ViewData["IsNumberReadOnly"]` jest udostępniane w widoku częściowego następujące:</span><span class="sxs-lookup"><span data-stu-id="2478c-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="2478c-141">W tym przykładzie wartość `ViewData["IsNumberReadOnly"]` Określa, czy *numer* pole jest wyświetlane jako tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="2478c-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2478c-142">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2478c-142">Additional resources</span></span>

* [<span data-ttu-id="2478c-143">Widoki częściowe</span><span class="sxs-lookup"><span data-stu-id="2478c-143">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="2478c-144">Lekko typu danych (ViewData, atrybut ViewData i obiekt ViewBag)</span><span class="sxs-lookup"><span data-stu-id="2478c-144">Weakly typed data (ViewData, ViewData attribute, and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag)
