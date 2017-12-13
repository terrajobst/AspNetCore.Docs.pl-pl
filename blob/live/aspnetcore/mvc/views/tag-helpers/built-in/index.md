---
title: "Wbudowane pomocników tagów platformy ASP.NET Core"
author: pkellner
description: "Wbudowane pomocników tagów platformy ASP.NET Core"
keywords: "Platformy ASP.NET Core pomocnika tagów"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: dd732822a715df19c0ee4b6accad3455ad6537da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="61af8-104">Wbudowane pomocników tagów platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61af8-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="61af8-105">Przez [Kellner Peterowi](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="61af8-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="61af8-106">Platformy ASP.NET Core zawiera wiele wbudowanych pomocników tagów w celu zwiększania wydajności pracy.</span><span class="sxs-lookup"><span data-stu-id="61af8-106">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="61af8-107">Ta sekcja zawiera omówienie wbudowanego pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="61af8-107">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="61af8-108">Dostępne są wbudowane pomocników tagów, które nie są już, ponieważ jest używana wewnętrznie przez [Razor](xref:mvc/views/razor) aparatu widoku.</span><span class="sxs-lookup"><span data-stu-id="61af8-108">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="61af8-109">Dotyczy to również pomocnika tagów dla ~ znak, który zwiększa się do ścieżki katalogu głównego witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="61af8-109">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="61af8-110">Wbudowane platformy ASP.NET Core pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="61af8-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="61af8-111">**[Pomocnik Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="61af8-112">**[Pamięć podręczna pomocnika tagów](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="61af8-113">**[Pomocnik Tag rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="61af8-114">**[Pomocnik Tag środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="61af8-115">**[Pomocnik Tag formularza](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-115">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="61af8-116">**[Pomocnik Tag obrazu](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-116">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="61af8-117">**[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-117">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="61af8-118">**[Etykieta pomocnika tagów](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-118">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="61af8-119">**[Wybierz pomocnika tagów](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="61af8-120">**[TextArea Tag pomocnika](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="61af8-121">**[Pomocnik Tag komunikatu weryfikacji](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="61af8-122">**[Pomocnik weryfikacji Summary — Tag](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="61af8-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61af8-123">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="61af8-123">Additional resources</span></span>

* [<span data-ttu-id="61af8-124">Programowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="61af8-124">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="61af8-125">Pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="61af8-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
