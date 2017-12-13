---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: "Wyłączanie akcje podczas animacji (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Obsługuje ona również akcji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 0f68d27f60e9b224a7cc0d598962553afeb3f28f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="disabling-actions-during-animation-vb"></a><span data-ttu-id="4ab26-104">Wyłączanie działania podczas animacji (VB)</span><span class="sxs-lookup"><span data-stu-id="4ab26-104">Disabling Actions during Animation (VB)</span></span>
====================
<span data-ttu-id="4ab26-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4ab26-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4ab26-106">[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4ab26-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span></span>

> <span data-ttu-id="4ab26-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="4ab26-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4ab26-108">Obsługuje ona również akcje, takie jak kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="4ab26-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="4ab26-109">Jednak po uruchomieniu animacji na kliknięcie myszą jest pożądane, aby wyłączyć kliknięć myszą podczas animacji.</span><span class="sxs-lookup"><span data-stu-id="4ab26-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="4ab26-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4ab26-110">Overview</span></span>

<span data-ttu-id="4ab26-111">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="4ab26-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4ab26-112">Obsługuje ona również akcje, takie jak kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="4ab26-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="4ab26-113">Jednak po uruchomieniu animacji na kliknięcie myszą jest pożądane, aby wyłączyć kliknięć myszą podczas animacji.</span><span class="sxs-lookup"><span data-stu-id="4ab26-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="4ab26-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="4ab26-114">Steps</span></span>

<span data-ttu-id="4ab26-115">Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="4ab26-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

<span data-ttu-id="4ab26-116">Animacji zostanie zastosowana do przycisku HTML następująco:</span><span class="sxs-lookup"><span data-stu-id="4ab26-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

<span data-ttu-id="4ab26-117">Należy pamiętać, że kontrolkę HTML jest używana zamiast formant sieci Web, ponieważ nie chcemy przycisk, aby utworzyć odświeżania strony; jest on tylko Uruchom animacji po stronie klienta dla instytucji.</span><span class="sxs-lookup"><span data-stu-id="4ab26-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="4ab26-118">Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="4ab26-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

<span data-ttu-id="4ab26-119">W ramach `<Animations>` węzła, `<OnClick>` jest elementem prawo do obsługi kliknięcia myszą.</span><span class="sxs-lookup"><span data-stu-id="4ab26-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="4ab26-120">Jednak przycisk można kliknięty podczas animacji, jak również.</span><span class="sxs-lookup"><span data-stu-id="4ab26-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="4ab26-121">`<EnableAction>` Zająć tego elementu.</span><span class="sxs-lookup"><span data-stu-id="4ab26-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="4ab26-122">Ustawienie `Enabled="false"` wyłącza przycisk jako część animacji.</span><span class="sxs-lookup"><span data-stu-id="4ab26-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="4ab26-123">Ponieważ używamy kilka poszczególnych animacji (wyłączenie przycisku i rzeczywistego animacje), `<Parallel>` element jest wymagany do przyklej pojedynczego animacji w jednej.</span><span class="sxs-lookup"><span data-stu-id="4ab26-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="4ab26-124">W tym miejscu jest pełny kod znaczników dla `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="4ab26-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

<span data-ttu-id="4ab26-125">Również będzie można ponownie włączyć do przycisku po animacji, przy użyciu następujących elementów XML na końcu listy:</span><span class="sxs-lookup"><span data-stu-id="4ab26-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

<span data-ttu-id="4ab26-126">Jednak w danym scenariuszu to bezużyteczne od przycisku znika i nie jest widoczny po zakończeniu animacji.</span><span class="sxs-lookup"><span data-stu-id="4ab26-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="4ab26-127">[![Przycisk jest wyłączona, jak działa animacji](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4ab26-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span></span>

<span data-ttu-id="4ab26-128">Przycisk jest wyłączona, jak działa animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](disabling-actions-during-animation-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4ab26-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4ab26-129">[Poprzednie](animating-in-response-to-user-interaction-vb.md)
[dalej](triggering-an-animation-in-another-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4ab26-129">[Previous](animating-in-response-to-user-interaction-vb.md)
[Next](triggering-an-animation-in-another-control-vb.md)</span></span>
