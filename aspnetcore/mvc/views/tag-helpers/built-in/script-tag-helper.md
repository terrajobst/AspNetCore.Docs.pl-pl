---
title: Pomocnik tagu skryptu w ASP.NET Core
author: rick-anderson
ms.author: riande
description: Odkryj atrybuty pomocnika tagów ASP.NET Core i rolę, jaką każdy atrybut odgrywa w rozszerzeniu zachowania tagu skryptu HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256499"
---
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="c52f4-103">Pomocnik tagu skryptu w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c52f4-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="c52f4-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c52f4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c52f4-105">[Pomocnik tagu skryptu](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generuje link do pliku skryptu podstawowego lub odwracania.</span><span class="sxs-lookup"><span data-stu-id="c52f4-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="c52f4-106">Zazwyczaj podstawowy plik skryptu znajduje się w [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span><span class="sxs-lookup"><span data-stu-id="c52f4-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="c52f4-107">Pomocnik tagu skryptu pozwala określić sieć CDN dla pliku skryptu i rezerwę, gdy sieć CDN nie jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="c52f4-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="c52f4-108">Pomocnik tagów skryptu zapewnia wydajność sieci CDN z niezawodną obsługą hostingu lokalnego.</span><span class="sxs-lookup"><span data-stu-id="c52f4-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="c52f4-109">Poniższy znacznik Razor pokazuje `script` element pliku układu utworzony za pomocą szablonu aplikacji sieci Web ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c52f4-109">The following Razor markup shows the `script` element of a layout file created with the ASP.NET Core web app template:</span></span>

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

<span data-ttu-id="c52f4-110">Poniżej przedstawiono podobne do renderowanego kodu HTML z poprzedniego kodu (w środowisku nieprogramistycznym):</span><span class="sxs-lookup"><span data-stu-id="c52f4-110">The following is similar to the rendered HTML from the preceding code (in a non-Development environment):</span></span>

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

<span data-ttu-id="c52f4-111">W poprzednim kodzie pomocnik tagów skryptu wygenerował drugi element skryptu ( `<script>  (window.jQuery || document.write(`), który `window.jQuery`testuje.</span><span class="sxs-lookup"><span data-stu-id="c52f4-111">In the preceding code, the Script Tag Helper generated the second script ( `<script>  (window.jQuery || document.write(`) element, which tests for `window.jQuery`.</span></span> <span data-ttu-id="c52f4-112">Jeśli `window.jQuery` nie zostanie znaleziona `document.write(` , uruchamia i tworzy skrypt</span><span class="sxs-lookup"><span data-stu-id="c52f4-112">If `window.jQuery` is not found, `document.write(` runs and creates a script</span></span> 

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="c52f4-113">Atrybuty pomocnika często używanych tagów skryptu</span><span class="sxs-lookup"><span data-stu-id="c52f4-113">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="c52f4-114">Zobacz [pomocnik tagów skryptów](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) dla wszystkich atrybutów pomocnika tagów skryptu, właściwości i metod.</span><span class="sxs-lookup"><span data-stu-id="c52f4-114">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="href"></a><span data-ttu-id="c52f4-115">{1&gt;href&lt;1}</span><span class="sxs-lookup"><span data-stu-id="c52f4-115">href</span></span>

<span data-ttu-id="c52f4-116">Preferowany adres połączonego zasobu.</span><span class="sxs-lookup"><span data-stu-id="c52f4-116">Preferred address of the linked resource.</span></span> <span data-ttu-id="c52f4-117">Adres jest przekazaniem do wygenerowanego kodu HTML we wszystkich przypadkach.</span><span class="sxs-lookup"><span data-stu-id="c52f4-117">The address is passed thought to the generated HTML in all cases.</span></span>

### <a name="asp-fallback-href"></a><span data-ttu-id="c52f4-118">ASP-Fallback-href</span><span class="sxs-lookup"><span data-stu-id="c52f4-118">asp-fallback-href</span></span>

<span data-ttu-id="c52f4-119">Adres URL arkusza stylów CSS, do którego ma nastąpić powrót w przypadku niepowodzenia podstawowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="c52f4-119">The URL of a CSS stylesheet to fallback to in the case the primary URL fails.</span></span>

### <a name="asp-fallback-test-class"></a><span data-ttu-id="c52f4-120">ASP-Fallback-test-Klasa</span><span class="sxs-lookup"><span data-stu-id="c52f4-120">asp-fallback-test-class</span></span>

<span data-ttu-id="c52f4-121">Nazwa klasy zdefiniowana w arkuszu stylów do użycia w teście Fallback.</span><span class="sxs-lookup"><span data-stu-id="c52f4-121">The class name defined in the stylesheet to use for the fallback test.</span></span> <span data-ttu-id="c52f4-122">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span><span class="sxs-lookup"><span data-stu-id="c52f4-122">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span></span>

### <a name="asp-fallback-test-property"></a><span data-ttu-id="c52f4-123">ASP-Fallback-test-Property</span><span class="sxs-lookup"><span data-stu-id="c52f4-123">asp-fallback-test-property</span></span>

<span data-ttu-id="c52f4-124">Nazwa właściwości CSS do użycia w teście Fallback.</span><span class="sxs-lookup"><span data-stu-id="c52f4-124">The CSS property name to use for the fallback test.</span></span> <span data-ttu-id="c52f4-125">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span><span class="sxs-lookup"><span data-stu-id="c52f4-125">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="c52f4-126">ASP — wartość testowa</span><span class="sxs-lookup"><span data-stu-id="c52f4-126">asp-fallback-test-value</span></span>

<span data-ttu-id="c52f4-127">Wartość właściwości CSS do użycia dla testu powrotu.</span><span class="sxs-lookup"><span data-stu-id="c52f4-127">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="c52f4-128">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span><span class="sxs-lookup"><span data-stu-id="c52f4-128">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="c52f4-129">ASP — wartość testowa</span><span class="sxs-lookup"><span data-stu-id="c52f4-129">asp-fallback-test-value</span></span>

<span data-ttu-id="c52f4-130">Wartość właściwości CSS do użycia dla testu powrotu.</span><span class="sxs-lookup"><span data-stu-id="c52f4-130">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="c52f4-131">Aby uzyskać więcej informacji, zobacz<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span><span class="sxs-lookup"><span data-stu-id="c52f4-131">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span></span>

## <a name="additional-resources"></a><span data-ttu-id="c52f4-132">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c52f4-132">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
