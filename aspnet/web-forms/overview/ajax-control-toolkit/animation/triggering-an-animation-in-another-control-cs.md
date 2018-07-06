---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Wyzwalanie animacji w innej kontrolce (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Ogólnie rzecz biorąc, uruchamianie...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a7eeb522ae982cd5a84aa9c8e4228871c8c78440
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810805"
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="57fc1-104">Wyzwalanie animacji w innej kontrolce (C#)</span><span class="sxs-lookup"><span data-stu-id="57fc1-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="57fc1-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="57fc1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="57fc1-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="57fc1-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="57fc1-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="57fc1-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="57fc1-108">Ogólnie rzecz biorąc uruchamianie animacji jest wyzwalana przez interakcji użytkownika z tej samej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="57fc1-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="57fc1-109">Jest jednak również możliwość oddziałują na jeden formant, a następnie animacji innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="57fc1-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="57fc1-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="57fc1-110">Overview</span></span>

<span data-ttu-id="57fc1-111">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="57fc1-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="57fc1-112">Ogólnie rzecz biorąc uruchamianie animacji jest wyzwalana przez interakcji użytkownika z tej samej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="57fc1-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="57fc1-113">Jest jednak również możliwość oddziałują na jeden formant, a następnie animacji innej kontrolki.</span><span class="sxs-lookup"><span data-stu-id="57fc1-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="57fc1-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="57fc1-114">Steps</span></span>

<span data-ttu-id="57fc1-115">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="57fc1-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="57fc1-116">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="57fc1-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="57fc1-117">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="57fc1-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="57fc1-118">Aby rozpocząć, animowanie panelu, przycisk HTML jest używany.</span><span class="sxs-lookup"><span data-stu-id="57fc1-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="57fc1-119">Należy pamiętać, że `<input type="button" />` jest uprzywilejowanym za pośrednictwem `<asp:Button />` ponieważ nie chcemy ogłaszania zwrotnego po kliknięciu tego przycisku przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="57fc1-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="57fc1-120">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="57fc1-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="57fc1-121">Jest ważne, aby ustawić `TargetControlID` identyfikator przycisku (element wyzwalanie animacji), nie identyfikatorowi panelu (elementu, jest animowany)</span><span class="sxs-lookup"><span data-stu-id="57fc1-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="57fc1-122">W ramach `<Animations>` węzła, animacje miejsce w zwykły sposób.</span><span class="sxs-lookup"><span data-stu-id="57fc1-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="57fc1-123">Aby stały się zmienić w panelu, ustaw nie przycisk `AnimationTarget` atrybutu dla każdego elementu animacji w ramach `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="57fc1-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="57fc1-124">Wartość `AnimationTarget` oczywiście jest to identyfikator panelu.</span><span class="sxs-lookup"><span data-stu-id="57fc1-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="57fc1-125">W ten sposób animacji się zdarzyć, za pomocą panelu, nie za pomocą przycisku wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="57fc1-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="57fc1-126">Oto `AnimationExtender` znaczników dla tego scenariusza:</span><span class="sxs-lookup"><span data-stu-id="57fc1-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="57fc1-127">Należy zwrócić uwagę specjalne kolejność, w jakiej są wyświetlane poszczególne animacji.</span><span class="sxs-lookup"><span data-stu-id="57fc1-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="57fc1-128">Po pierwsze przycisk pobiera dezaktywowane, po uruchomieniu animacji.</span><span class="sxs-lookup"><span data-stu-id="57fc1-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="57fc1-129">Ponieważ ma nie `AnimationTarget` atrybutu w `<EnableAction>` elementu, ta animacja jest stosowana do sterowania źródłowy: przycisku.</span><span class="sxs-lookup"><span data-stu-id="57fc1-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="57fc1-130">Kroki opisane w dwóch następnych animacji są przeprowadzane parallelly (`<Parallel>` elementu).</span><span class="sxs-lookup"><span data-stu-id="57fc1-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="57fc1-131">Zachowują się ich `AnimationTarget` Ustaw atrybuty `"Panel1"`, dlatego animowanie panelu nie przycisk.</span><span class="sxs-lookup"><span data-stu-id="57fc1-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="57fc1-132">[![Kliknięcie na przycisk uruchamiania animacji panelu](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="57fc1-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="57fc1-133">Kliknięcie na przycisk uruchamia panel animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="57fc1-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="57fc1-134">[Poprzednie](disabling-actions-during-animation-cs.md)
> [dalej](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="57fc1-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
