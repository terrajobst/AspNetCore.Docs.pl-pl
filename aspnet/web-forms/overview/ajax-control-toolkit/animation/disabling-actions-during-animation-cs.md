---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Wyłączanie akcji podczas animacji (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Obsługuje ona również działanie...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 396411332735503d1781e171bead8ab6aaed6efa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828198"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="d639b-104">Wyłączanie akcji podczas animacji (C#)</span><span class="sxs-lookup"><span data-stu-id="d639b-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="d639b-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d639b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d639b-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d639b-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="d639b-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="d639b-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d639b-108">Obsługuje ona również akcje, takie jak kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="d639b-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="d639b-109">Jednak po uruchomieniu przez kliknięcie myszą animacji jest pożądane, aby wyłączyć kliknięć myszą podczas animacji.</span><span class="sxs-lookup"><span data-stu-id="d639b-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="d639b-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d639b-110">Overview</span></span>

<span data-ttu-id="d639b-111">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="d639b-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d639b-112">Obsługuje ona również akcje, takie jak kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="d639b-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="d639b-113">Jednak po uruchomieniu przez kliknięcie myszą animacji jest pożądane, aby wyłączyć kliknięć myszą podczas animacji.</span><span class="sxs-lookup"><span data-stu-id="d639b-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="d639b-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="d639b-114">Steps</span></span>

<span data-ttu-id="d639b-115">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="d639b-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="d639b-116">Animacja zostaną zastosowane do przycisku HTML następująco:</span><span class="sxs-lookup"><span data-stu-id="d639b-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="d639b-117">Należy pamiętać, że formant HTML jest używana zamiast formantu sieci Web, ponieważ nie chcemy przycisk, aby utworzyć zwrotu; po prostu są to uruchomienie animacji po stronie klienta dla nas.</span><span class="sxs-lookup"><span data-stu-id="d639b-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="d639b-118">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="d639b-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="d639b-119">W ramach `<Animations>` węzła `<OnClick>` jest odpowiednich elementów do obsługi kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="d639b-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="d639b-120">Jednak przycisku można kliknął podczas animacji, jak również.</span><span class="sxs-lookup"><span data-stu-id="d639b-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="d639b-121">`<EnableAction>` Elementu może zajmować się tego.</span><span class="sxs-lookup"><span data-stu-id="d639b-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="d639b-122">Ustawienie `Enabled="false"` wyłącza przycisk jako część animacji.</span><span class="sxs-lookup"><span data-stu-id="d639b-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="d639b-123">Ponieważ używamy kilku poszczególnych animacji (wyłączenie przycisku i rzeczywiste animacji), `<Parallel>` element jest wymagany do przyklej jednej animacji ze sobą w jednym.</span><span class="sxs-lookup"><span data-stu-id="d639b-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="d639b-124">Oto kompletny kod znaczników dla `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="d639b-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="d639b-125">Byłoby to możliwe ponownie włączyć przycisk po animacji, przy użyciu następujących elementów XML na końcu listy:</span><span class="sxs-lookup"><span data-stu-id="d639b-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="d639b-126">Jednak w danym scenariuszu byłoby to bezcelowe od przycisku znika i nie jest widoczny na końcu animacji.</span><span class="sxs-lookup"><span data-stu-id="d639b-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="d639b-127">[![Przycisk został wyłączony, tak szybko, jak działa animacji](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d639b-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="d639b-128">Przycisk został wyłączony, tak szybko, jak działa animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d639b-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d639b-129">[Poprzednie](animating-in-response-to-user-interaction-cs.md)
> [dalej](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d639b-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
