---
title: Wbudowane pomocników tagów platformy ASP.NET Core
author: pkellner
description: Dowiedz się, jak wbudowane pomocników tagów platformy ASP.NET Core zwiększania wydajności pracy.
ms.author: riande
ms.date: 09/13/2017
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 4f2ebf1600f42847db1c1f9517787b020d2e86c9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279171"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="2a3e0-103">Wbudowane pomocników tagów platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a3e0-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="2a3e0-104">Przez [Kellner Peterowi](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="2a3e0-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="2a3e0-105">Platformy ASP.NET Core zawiera wiele wbudowanych pomocników tagów w celu zwiększania wydajności pracy.</span><span class="sxs-lookup"><span data-stu-id="2a3e0-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="2a3e0-106">Ta sekcja zawiera omówienie wbudowanego pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="2a3e0-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="2a3e0-107">Dostępne są wbudowane pomocników tagów, które nie są już, ponieważ jest używana wewnętrznie przez [Razor](xref:mvc/views/razor) aparatu widoku.</span><span class="sxs-lookup"><span data-stu-id="2a3e0-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="2a3e0-108">Dotyczy to również pomocnika tagów dla ~ znak, który zwiększa się do ścieżki katalogu głównego witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2a3e0-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="2a3e0-109">Wbudowane platformy ASP.NET Core pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="2a3e0-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="2a3e0-110">**[Pomocnik Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="2a3e0-111">**[Pamięć podręczna pomocnika tagów](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="2a3e0-112">**[Pomocnik Tag rozproszonej pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="2a3e0-113">**[Pomocnik Tag środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="2a3e0-114">**[Pomocnik Tag formularza](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="2a3e0-115">**[Pomocnik Tag obrazu](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="2a3e0-116">**[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="2a3e0-117">**[Etykieta pomocnika tagów](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="2a3e0-118">**[Pomocnik Tag częściowe](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="2a3e0-119">**[Wybierz pomocnika tagów](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="2a3e0-120">**[TextArea Tag pomocnika](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="2a3e0-121">**[Pomocnik Tag komunikatu weryfikacji](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="2a3e0-122">**[Pomocnik weryfikacji Summary — Tag](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a3e0-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2a3e0-123">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2a3e0-123">Additional resources</span></span>

* [<span data-ttu-id="2a3e0-124">Programowanie po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="2a3e0-124">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="2a3e0-125">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="2a3e0-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
