---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: "Wyłączanie akcje podczas animacji (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Obsługuje ona również akcji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 875c6be5e190c31fac030fc17ef040a934233f16
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="83741-104">Wyłączanie działania podczas animacji (C#)</span><span class="sxs-lookup"><span data-stu-id="83741-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="83741-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="83741-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="83741-106">[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="83741-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="83741-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="83741-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="83741-108">Obsługuje ona również akcje, takie jak kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="83741-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="83741-109">Jednak po uruchomieniu animacji na kliknięcie myszą jest pożądane, aby wyłączyć kliknięć myszą podczas animacji.</span><span class="sxs-lookup"><span data-stu-id="83741-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="83741-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="83741-110">Overview</span></span>

<span data-ttu-id="83741-111">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="83741-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="83741-112">Obsługuje ona również akcje, takie jak kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="83741-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="83741-113">Jednak po uruchomieniu animacji na kliknięcie myszą jest pożądane, aby wyłączyć kliknięć myszą podczas animacji.</span><span class="sxs-lookup"><span data-stu-id="83741-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="83741-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="83741-114">Steps</span></span>

<span data-ttu-id="83741-115">Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="83741-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="83741-116">Animacji zostanie zastosowana do przycisku HTML następująco:</span><span class="sxs-lookup"><span data-stu-id="83741-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="83741-117">Należy pamiętać, że kontrolkę HTML jest używana zamiast formant sieci Web, ponieważ nie chcemy przycisk, aby utworzyć odświeżania strony; jest on tylko Uruchom animacji po stronie klienta dla instytucji.</span><span class="sxs-lookup"><span data-stu-id="83741-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="83741-118">Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="83741-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="83741-119">W ramach `<Animations>` węzła, `<OnClick>` jest elementem prawo do obsługi kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="83741-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="83741-120">Jednak przycisk można kliknięty podczas animacji, jak również.</span><span class="sxs-lookup"><span data-stu-id="83741-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="83741-121">`<EnableAction>` Zająć tego elementu.</span><span class="sxs-lookup"><span data-stu-id="83741-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="83741-122">Ustawienie `Enabled="false"` wyłącza przycisk jako część animacji.</span><span class="sxs-lookup"><span data-stu-id="83741-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="83741-123">Ponieważ używamy kilka poszczególnych animacji (wyłączenie przycisku i rzeczywistego animacje), `<Parallel>` element jest wymagany do przyklej pojedynczego animacji w jednej.</span><span class="sxs-lookup"><span data-stu-id="83741-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="83741-124">W tym miejscu jest pełny kod znaczników dla `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="83741-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="83741-125">Również będzie można ponownie włączyć do przycisku po animacji, przy użyciu następujących elementów XML na końcu listy:</span><span class="sxs-lookup"><span data-stu-id="83741-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="83741-126">Jednak w danym scenariuszu to bezużyteczne od przycisku znika i nie jest widoczny po zakończeniu animacji.</span><span class="sxs-lookup"><span data-stu-id="83741-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="83741-127">[![Przycisk jest wyłączona, jak działa animacji](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="83741-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="83741-128">Przycisk jest wyłączona, jak działa animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="83741-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="83741-129">[Poprzednie](animating-in-response-to-user-interaction-cs.md)
[dalej](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="83741-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
