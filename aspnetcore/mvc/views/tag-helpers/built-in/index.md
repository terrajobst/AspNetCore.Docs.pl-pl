---
title: Pomocnicy tagów wbudowanych w platformy ASP.NET Core
author: pkellner
description: Dowiedz się, jak pomocnicy tagów wbudowanych w platformy ASP.NET Core zwiększyć produktywność.
ms.author: riande
ms.date: 09/18/2018
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 5d9425e0b68578c86a6f9a691322e0bb63a860fb
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292313"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="12ead-103">Pomocnicy tagów wbudowanych w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12ead-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="12ead-104">Przez [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="12ead-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="12ead-105">Platforma ASP.NET Core zawiera wiele wbudowanych pomocnicy tagów, aby zwiększyć produktywność.</span><span class="sxs-lookup"><span data-stu-id="12ead-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="12ead-106">Ta sekcja zawiera omówienie wbudowanego pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="12ead-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="12ead-107">Istnieją wbudowane pomocnicy tagów, które nie są omówione, ponieważ są używane wewnętrznie przez [Razor](xref:mvc/views/razor) aparatu widoku.</span><span class="sxs-lookup"><span data-stu-id="12ead-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="12ead-108">Obejmuje to pomocnika tagów dla ~ znak, który jest rozszerzany, aby ścieżka katalogu głównego witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="12ead-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="12ead-109">Pomocnicy tagów wbudowanych platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12ead-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="12ead-110">**[Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="12ead-111">**[Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="12ead-112">**[Pomocnik tagu rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="12ead-113">**[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="12ead-114">**[Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="12ead-115">**[Pomocnik tagu obrazu](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="12ead-116">**[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="12ead-117">**[Pomocnik tagu etykiet](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="12ead-118">**[Pomocnik tagu częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="12ead-119">**[Pomocnik tagu SELECT](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="12ead-120">**[Pomocnik tagu TextArea](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="12ead-121">**[Pomocnik tagu komunikat sprawdzania poprawności](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="12ead-122">**[Pomocnik tagu podsumowania sprawdzania poprawności](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="12ead-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12ead-123">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="12ead-123">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>
