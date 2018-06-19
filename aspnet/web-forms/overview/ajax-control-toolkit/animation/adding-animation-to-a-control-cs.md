---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Dodawanie animacji do formantu (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Ten samouczek pokazuje, jak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ba122660045c3f5dd4b11f118df174a79de814a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872139"
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="c50af-104">Dodawanie animacji do formantu (C#)</span><span class="sxs-lookup"><span data-stu-id="c50af-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="c50af-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c50af-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c50af-106">[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c50af-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="c50af-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="c50af-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c50af-108">Ten samouczek pokazuje, jak skonfigurować takie animacji.</span><span class="sxs-lookup"><span data-stu-id="c50af-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="c50af-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c50af-109">Overview</span></span>

<span data-ttu-id="c50af-110">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="c50af-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c50af-111">Ten samouczek pokazuje, jak skonfigurować takie animacji.</span><span class="sxs-lookup"><span data-stu-id="c50af-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="c50af-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="c50af-112">Steps</span></span>

<span data-ttu-id="c50af-113">Pierwszym krokiem jest normalnie obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX została załadowana i można go używać zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="c50af-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="c50af-114">Animacja w tym scenariuszu zostaną zastosowane do panelu tekstu, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="c50af-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="c50af-115">Skojarzona klasa CSS w panelu definiuje kolor tła i szerokości:</span><span class="sxs-lookup"><span data-stu-id="c50af-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="c50af-116">Następnie up, potrzebujemy `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="c50af-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="c50af-117">Po podaniu `ID` i zwykle `runat="server"`, `TargetControlID` atrybut musi mieć ustawioną formantu animacji, w tym przypadku panelu:</span><span class="sxs-lookup"><span data-stu-id="c50af-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="c50af-118">Całe zastosowania animacji deklaratywnie, używając składni XML, Niestety obecnie nie są w pełni obsługiwane przez funkcję IntelliSense programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c50af-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="c50af-119">Węzeł główny jest `<Animations>;` w tym węźle kilka zdarzeń mogą ustalić, kiedy animacje zakończeniu miejscu:</span><span class="sxs-lookup"><span data-stu-id="c50af-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="c50af-120">`OnClick` (kliknij przycisk myszy)</span><span class="sxs-lookup"><span data-stu-id="c50af-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="c50af-121">`OnHoverOut` (gdy mysz opuści formantu)</span><span class="sxs-lookup"><span data-stu-id="c50af-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="c50af-122">`OnHoverOver` (gdy mysz znajduje się nad formantem, zatrzymywanie `OnHoverOut` animacji)</span><span class="sxs-lookup"><span data-stu-id="c50af-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="c50af-123">`OnLoad` (po załadowaniu strony)</span><span class="sxs-lookup"><span data-stu-id="c50af-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="c50af-124">`OnMouseOut` (gdy mysz opuści formantu)</span><span class="sxs-lookup"><span data-stu-id="c50af-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="c50af-125">`OnMouseOver` (gdy mysz znajduje się nad formantem, nie zatrzymuje `OnMouseOut` animacji)</span><span class="sxs-lookup"><span data-stu-id="c50af-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="c50af-126">Platformę jest dostarczany z zestawem animacji, każdy z nich reprezentowany przez jego własnej — element XML.</span><span class="sxs-lookup"><span data-stu-id="c50af-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="c50af-127">Oto zaznaczenia:</span><span class="sxs-lookup"><span data-stu-id="c50af-127">Here is a selection:</span></span>

- <span data-ttu-id="c50af-128">`<Color>` (zmiana koloru)</span><span class="sxs-lookup"><span data-stu-id="c50af-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="c50af-129">`<FadeIn>` (zanikania)</span><span class="sxs-lookup"><span data-stu-id="c50af-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="c50af-130">`<FadeOut>` (wygaszanie)</span><span class="sxs-lookup"><span data-stu-id="c50af-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="c50af-131">`<Property>` (zmiana właściwości formantu)</span><span class="sxs-lookup"><span data-stu-id="c50af-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="c50af-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="c50af-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="c50af-133">`<Resize>` (zmiana rozmiaru)</span><span class="sxs-lookup"><span data-stu-id="c50af-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="c50af-134">`<Scale>` (proporcjonalnie zmiany rozmiaru)</span><span class="sxs-lookup"><span data-stu-id="c50af-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="c50af-135">W tym przykładzie panelu są zanikania. Animacja podejmują 1,5 s (`Duration` atrybut), wyświetlanie 24 ramki (procedura animacji) na sekundę (`Fps` attributs).</span><span class="sxs-lookup"><span data-stu-id="c50af-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="c50af-136">W tym miejscu jest pełny kod znaczników dla `AnimationExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="c50af-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="c50af-137">Po uruchomieniu tego skryptu, panel jest wyświetlany i stopniowo zmniejsza się w jednym i pół sekundy.</span><span class="sxs-lookup"><span data-stu-id="c50af-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="c50af-138">[![Wygaszanie jest panelu](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c50af-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="c50af-139">Wygaszanie jest panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c50af-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c50af-140">Next</span><span class="sxs-lookup"><span data-stu-id="c50af-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
