---
title: "Wbudowane pomocników tagów platformy ASP.NET Core"
author: pkellner
description: "Wbudowane pomocników tagów platformy ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 8ffc748ec3d4eed35871543f5ceccc86aadee661
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="b64e9-103">Wbudowane pomocników tagów platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b64e9-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="b64e9-104">Przez [Kellner Peterowi](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="b64e9-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="b64e9-105">Platformy ASP.NET Core zawiera wiele wbudowanych pomocników tagów w celu zwiększania wydajności pracy.</span><span class="sxs-lookup"><span data-stu-id="b64e9-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="b64e9-106">Ta sekcja zawiera omówienie wbudowanego pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="b64e9-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="b64e9-107">Dostępne są wbudowane pomocników tagów, które nie są już, ponieważ jest używana wewnętrznie przez [Razor](xref:mvc/views/razor) aparatu widoku.</span><span class="sxs-lookup"><span data-stu-id="b64e9-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="b64e9-108">Dotyczy to również pomocnika tagów dla ~ znak, który zwiększa się do ścieżki katalogu głównego witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b64e9-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="b64e9-109">Wbudowane platformy ASP.NET Core pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="b64e9-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="b64e9-110">**[Pomocnik Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="b64e9-111">**[Pamięć podręczna pomocnika tagów](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="b64e9-112">**[Pomocnik Tag rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="b64e9-113">**[Pomocnik Tag środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="b64e9-114">**[Pomocnik Tag formularza](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="b64e9-115">**[Pomocnik Tag obrazu](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="b64e9-116">**[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="b64e9-117">**[Etykieta pomocnika tagów](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="b64e9-118">**[Wybierz pomocnika tagów](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-118">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="b64e9-119">**[TextArea Tag pomocnika](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-119">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="b64e9-120">**[Pomocnik Tag komunikatu weryfikacji](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-120">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="b64e9-121">**[Pomocnik weryfikacji Summary — Tag](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b64e9-121">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b64e9-122">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b64e9-122">Additional resources</span></span>

* [<span data-ttu-id="b64e9-123">Programowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="b64e9-123">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="b64e9-124">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="b64e9-124">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
