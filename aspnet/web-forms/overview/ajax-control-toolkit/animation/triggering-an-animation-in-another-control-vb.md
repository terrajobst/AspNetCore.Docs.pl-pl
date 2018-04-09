---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Wyzwolenie animacji w innym formantu (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Ogólnie rzecz biorąc, uruchamianie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 262a17e7521a8ea16c81e8dfdc6d3b6614c18eea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="0a7cb-104">Wyzwolenie animacji w innym formantu (VB)</span><span class="sxs-lookup"><span data-stu-id="0a7cb-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="0a7cb-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0a7cb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0a7cb-106">[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0a7cb-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="0a7cb-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0a7cb-108">Ogólnie rzecz biorąc uruchomienie animacji zostanie wywołany przez użytkownika za pomocą formantu tego samego.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="0a7cb-109">Jest jednak również możliwość interakcji z formantami, a następnie animacji inny formant.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="0a7cb-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="0a7cb-110">Overview</span></span>

<span data-ttu-id="0a7cb-111">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0a7cb-112">Ogólnie rzecz biorąc uruchomienie animacji zostanie wywołany przez użytkownika za pomocą formantu tego samego.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="0a7cb-113">Jest jednak również możliwość interakcji z formantami, a następnie animacji inny formant.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="0a7cb-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="0a7cb-114">Steps</span></span>

<span data-ttu-id="0a7cb-115">Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="0a7cb-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="0a7cb-116">Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="0a7cb-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="0a7cb-117">Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="0a7cb-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="0a7cb-118">Aby rozpocząć animacji panelu, przycisk HTML jest używany.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="0a7cb-119">Należy pamiętać, że `<input type="button" />` jest uprzywilejowanym za pośrednictwem `<asp:Button />` ponieważ nie chcemy odświeżania strony po kliknięciu tego przycisku.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="0a7cb-120">Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="0a7cb-121">Ważne jest, aby ustawić `TargetControlID` identyfikator przycisku (element wyzwalania animacji), nie identyfikatorowi panelu (element animowanej)</span><span class="sxs-lookup"><span data-stu-id="0a7cb-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="0a7cb-122">W ramach `<Animations>` węzła, animacje miejsce w zwykły sposób.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="0a7cb-123">Aby je zmienić panelu, ustaw nie przycisk `AnimationTarget` atrybutu dla każdego elementu animacji w `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="0a7cb-124">Wartość `AnimationTarget` oczywiście jest Identyfikatorem panelu.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="0a7cb-125">W ten sposób animacji się tak zdarzyć przy użyciu panelu, nie z przyciskiem wyzwalająca.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="0a7cb-126">Oto `AnimationExtender` znaczników dla tego scenariusza:</span><span class="sxs-lookup"><span data-stu-id="0a7cb-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="0a7cb-127">Należy zwrócić uwagę specjalne kolejność poszczególnych animacji.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="0a7cb-128">Przede wszystkim przycisku pobiera dezaktywowane, po uruchomieniu animacji.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="0a7cb-129">Ponieważ nie `AnimationTarget` atrybutu w `<EnableAction>` elementu, to animacji jest stosowana do formantu źródłowego: przycisku.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="0a7cb-130">Animacja następne dwa kroki przeprowadza się parallelly (`<Parallel>` elementu).</span><span class="sxs-lookup"><span data-stu-id="0a7cb-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="0a7cb-131">Mają ich `AnimationTarget` ustawić atrybuty `"Panel1"`, w związku z tym animacji panelu, a nie przycisk.</span><span class="sxs-lookup"><span data-stu-id="0a7cb-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="0a7cb-132">[![Kliknięcie przycisku powoduje uruchomienie animacji panelu](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0a7cb-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="0a7cb-133">Kliknięcie przycisku powoduje uruchomienie animacji panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0a7cb-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0a7cb-134">[Poprzednie](disabling-actions-during-animation-vb.md)
> [dalej](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0a7cb-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
