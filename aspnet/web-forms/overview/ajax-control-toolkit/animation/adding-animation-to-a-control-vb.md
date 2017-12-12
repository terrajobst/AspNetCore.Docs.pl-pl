---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Dodawanie animacji do formantu (VB) | Dokumentacja firmy Microsoft
author: wenz
description: "Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Ten samouczek pokazuje, jak..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c2d6971ade89405245c8d23cafb6fd8bb9468639
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="e49b4-104">Dodawanie animacji do formantu (VB)</span><span class="sxs-lookup"><span data-stu-id="e49b4-104">Adding Animation to a Control (VB)</span></span>
====================
<span data-ttu-id="e49b4-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e49b4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e49b4-106">[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e49b4-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="e49b4-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="e49b4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e49b4-108">Ten samouczek pokazuje, jak skonfigurować takie animacji.</span><span class="sxs-lookup"><span data-stu-id="e49b4-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="e49b4-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="e49b4-109">Overview</span></span>

<span data-ttu-id="e49b4-110">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="e49b4-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e49b4-111">Ten samouczek pokazuje, jak skonfigurować takie animacji.</span><span class="sxs-lookup"><span data-stu-id="e49b4-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="e49b4-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="e49b4-112">Steps</span></span>

<span data-ttu-id="e49b4-113">Pierwszym krokiem jest normalnie obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX została załadowana i można go używać zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="e49b4-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="e49b4-114">Animacja w tym scenariuszu zostaną zastosowane do panelu tekstu, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="e49b4-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="e49b4-115">Skojarzona klasa CSS w panelu definiuje kolor tła i szerokości:</span><span class="sxs-lookup"><span data-stu-id="e49b4-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="e49b4-116">Następnie up, potrzebujemy `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="e49b4-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="e49b4-117">Po podaniu `ID` i zwykle `runat="server"`, `TargetControlID` atrybut musi mieć ustawioną formantu animacji, w tym przypadku panelu:</span><span class="sxs-lookup"><span data-stu-id="e49b4-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="e49b4-118">Całe zastosowania animacji deklaratywnie, używając składni XML, Niestety obecnie nie są w pełni obsługiwane przez funkcję IntelliSense programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e49b4-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="e49b4-119">Węzeł główny jest `<Animations>;` w tym węźle kilka zdarzeń mogą ustalić, kiedy animacje zakończeniu miejscu:</span><span class="sxs-lookup"><span data-stu-id="e49b4-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="e49b4-120">`OnClick`(kliknij przycisk myszy)</span><span class="sxs-lookup"><span data-stu-id="e49b4-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="e49b4-121">`OnHoverOut`(gdy mysz opuści formantu)</span><span class="sxs-lookup"><span data-stu-id="e49b4-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="e49b4-122">`OnHoverOver`(gdy mysz znajduje się nad formantem, zatrzymywanie `OnHoverOut` animacji)</span><span class="sxs-lookup"><span data-stu-id="e49b4-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="e49b4-123">`OnLoad`(po załadowaniu strony)</span><span class="sxs-lookup"><span data-stu-id="e49b4-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="e49b4-124">`OnMouseOut`(gdy mysz opuści formantu)</span><span class="sxs-lookup"><span data-stu-id="e49b4-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="e49b4-125">`OnMouseOver`(gdy mysz znajduje się nad formantem, nie zatrzymuje `OnMouseOut` animacji)</span><span class="sxs-lookup"><span data-stu-id="e49b4-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="e49b4-126">Platformę jest dostarczany z zestawem animacji, każdy z nich reprezentowany przez jego własnej — element XML.</span><span class="sxs-lookup"><span data-stu-id="e49b4-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="e49b4-127">Oto zaznaczenia:</span><span class="sxs-lookup"><span data-stu-id="e49b4-127">Here is a selection:</span></span>

- <span data-ttu-id="e49b4-128">`<Color>`(zmiana koloru)</span><span class="sxs-lookup"><span data-stu-id="e49b4-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="e49b4-129">`<FadeIn>`(zanikania)</span><span class="sxs-lookup"><span data-stu-id="e49b4-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="e49b4-130">`<FadeOut>`(wygaszanie)</span><span class="sxs-lookup"><span data-stu-id="e49b4-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="e49b4-131">`<Property>`(zmiana właściwości formantu)</span><span class="sxs-lookup"><span data-stu-id="e49b4-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="e49b4-132">`<Pulse>`(pulsating)</span><span class="sxs-lookup"><span data-stu-id="e49b4-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="e49b4-133">`<Resize>`(zmiana rozmiaru)</span><span class="sxs-lookup"><span data-stu-id="e49b4-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="e49b4-134">`<Scale>`(proporcjonalnie zmiany rozmiaru)</span><span class="sxs-lookup"><span data-stu-id="e49b4-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="e49b4-135">W tym przykładzie panelu są zanikania. Animacja podejmują 1,5 s (`Duration` atrybut), wyświetlanie 24 ramki (procedura animacji) na sekundę (`Fps` attributs).</span><span class="sxs-lookup"><span data-stu-id="e49b4-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="e49b4-136">W tym miejscu jest pełny kod znaczników dla `AnimationExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="e49b4-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="e49b4-137">Po uruchomieniu tego skryptu, panel jest wyświetlany i stopniowo zmniejsza się w jednym i pół sekundy.</span><span class="sxs-lookup"><span data-stu-id="e49b4-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="e49b4-138">[![Wygaszanie jest panelu](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e49b4-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="e49b4-139">Wygaszanie jest panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e49b4-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e49b4-140">[Poprzednie](dynamically-controlling-updatepanel-animations-cs.md)
[dalej](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e49b4-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
