---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Dodawanie animacji do kontrolki (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Ten samouczek pokazuje, jak...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 10e541bc7e8a9a72d27909d7b37cfaf1860fe369
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832953"
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="91084-104">Dodawanie animacji do kontrolki (C#)</span><span class="sxs-lookup"><span data-stu-id="91084-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="91084-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="91084-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="91084-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="91084-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="91084-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="91084-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="91084-108">W tym samouczku pokazano, jak skonfigurować takie animacji.</span><span class="sxs-lookup"><span data-stu-id="91084-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="91084-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="91084-109">Overview</span></span>

<span data-ttu-id="91084-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="91084-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="91084-111">W tym samouczku pokazano, jak skonfigurować takie animacji.</span><span class="sxs-lookup"><span data-stu-id="91084-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="91084-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="91084-112">Steps</span></span>

<span data-ttu-id="91084-113">Pierwszym krokiem jest jak zwykle obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX jest ładowany i można go używać razem sterowania:</span><span class="sxs-lookup"><span data-stu-id="91084-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="91084-114">Animacji w tym scenariuszu zostaną zastosowane do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="91084-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="91084-115">Skojarzona klasa CSS w panelu definiuje kolor tła i szerokości:</span><span class="sxs-lookup"><span data-stu-id="91084-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="91084-116">Następnie up, potrzebujemy `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="91084-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="91084-117">Po podaniu `ID` i zwykle `runat="server"`, `TargetControlID` atrybutu musi być równa kontrolki animacji w naszym przypadku panelu:</span><span class="sxs-lookup"><span data-stu-id="91084-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="91084-118">Całe zastosowania animacji deklaratywne, przy użyciu składni XML, Niestety obecnie nie są w pełni obsługiwane przez funkcję IntelliSense programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="91084-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="91084-119">Węzeł główny jest `<Animations>;` w tym węźle kilka zdarzeń są dozwolone, które określają, kiedy animation(s) zakończeniu miejsce:</span><span class="sxs-lookup"><span data-stu-id="91084-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="91084-120">`OnClick` (kliknięcie myszą)</span><span class="sxs-lookup"><span data-stu-id="91084-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="91084-121">`OnHoverOut` (Jeśli kursora myszy poza formant)</span><span class="sxs-lookup"><span data-stu-id="91084-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="91084-122">`OnHoverOver` (gdy wskaźnika myszy nad formant, zatrzymanie `OnHoverOut` animacji)</span><span class="sxs-lookup"><span data-stu-id="91084-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="91084-123">`OnLoad` (po stronie został załadowany)</span><span class="sxs-lookup"><span data-stu-id="91084-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="91084-124">`OnMouseOut` (Jeśli kursora myszy poza formant)</span><span class="sxs-lookup"><span data-stu-id="91084-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="91084-125">`OnMouseOver` (gdy wskaźnika myszy nad formant, nie zatrzymywanie `OnMouseOut` animacji)</span><span class="sxs-lookup"><span data-stu-id="91084-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="91084-126">Struktura zawiera zestaw animacji, każdy z nich reprezentowany przez własną — element XML.</span><span class="sxs-lookup"><span data-stu-id="91084-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="91084-127">Oto zaznaczenia:</span><span class="sxs-lookup"><span data-stu-id="91084-127">Here is a selection:</span></span>

- <span data-ttu-id="91084-128">`<Color>` (zmiana koloru)</span><span class="sxs-lookup"><span data-stu-id="91084-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="91084-129">`<FadeIn>` (rozjaśnianie)</span><span class="sxs-lookup"><span data-stu-id="91084-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="91084-130">`<FadeOut>` (wygaszanie)</span><span class="sxs-lookup"><span data-stu-id="91084-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="91084-131">`<Property>` (zmiana właściwości kontrolki)</span><span class="sxs-lookup"><span data-stu-id="91084-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="91084-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="91084-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="91084-133">`<Resize>` (zmiana rozmiaru)</span><span class="sxs-lookup"><span data-stu-id="91084-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="91084-134">`<Scale>` (proporcjonalnie zmiany rozmiaru)</span><span class="sxs-lookup"><span data-stu-id="91084-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="91084-135">W tym przykładzie panelu są zanikanie. Animacja podejmują 1,5 s (`Duration` atrybut), wyświetlanie 24 ramek (kroki animację) na sekundę (`Fps` attributs).</span><span class="sxs-lookup"><span data-stu-id="91084-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="91084-136">Oto kompletny kod znaczników dla `AnimationExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="91084-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="91084-137">Po uruchomieniu tego skryptu, panel jest wyświetlany i stopniowo zmniejsza się w ciągu półtora sekund.</span><span class="sxs-lookup"><span data-stu-id="91084-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="91084-138">[![Panel jest Ściemnianie](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="91084-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="91084-139">Panel jest wygaszanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="91084-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="91084-140">Next</span><span class="sxs-lookup"><span data-stu-id="91084-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
