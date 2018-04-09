---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animacja w zależności od stanu (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Określa, czy animacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: b530239e76654bc68a8fa6ac900a20df1d5699b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="7e4a3-104">Animacja w zależności od stanu (C#)</span><span class="sxs-lookup"><span data-stu-id="7e4a3-104">Animation Depending On a Condition (C#)</span></span>
====================
<span data-ttu-id="7e4a3-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7e4a3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7e4a3-106">[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7e4a3-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="7e4a3-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="7e4a3-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7e4a3-108">Określa, czy animacja jest uruchomić lub nie można również są zależne od warunku w postaci kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7e4a3-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="7e4a3-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="7e4a3-109">Overview</span></span>

<span data-ttu-id="7e4a3-110">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="7e4a3-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7e4a3-111">Określa, czy animacja jest uruchomić lub nie można również są zależne od warunku w postaci kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7e4a3-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="7e4a3-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="7e4a3-112">Steps</span></span>

<span data-ttu-id="7e4a3-113">Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="7e4a3-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="7e4a3-114">Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="7e4a3-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="7e4a3-115">Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="7e4a3-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="7e4a3-116">Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="7e4a3-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="7e4a3-117">W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchomienia animacji po całkowitym załadowaniem strony.</span><span class="sxs-lookup"><span data-stu-id="7e4a3-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="7e4a3-118">Zamiast jedną z animacji regularne `<Condition>` element wejścia play.</span><span class="sxs-lookup"><span data-stu-id="7e4a3-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="7e4a3-119">Kod JavaScript podany jako wartość `ConditionScript` atrybutu jest wykonywana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="7e4a3-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="7e4a3-120">Jeśli go daje w wyniku wartość true, animacji jest wykonywane, w przeciwnym razie nie.</span><span class="sxs-lookup"><span data-stu-id="7e4a3-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="7e4a3-121">Następujący kod zawiera dwa animacji, każde z nich jest wykonywana w przypadkach na losowe 50%.</span><span class="sxs-lookup"><span data-stu-id="7e4a3-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="7e4a3-122">Ponieważ może istnieć tylko jeden animacji w `<OnLoad>`, dwa `<Condition>` animacje są łączone ze sobą za pomocą `<Sequence>` elementu:</span><span class="sxs-lookup"><span data-stu-id="7e4a3-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="7e4a3-123">Należy pamiętać, że znak mniejszości (`<`) w `ConditionScript` atrybut musi być zmieniony ().</span><span class="sxs-lookup"><span data-stu-id="7e4a3-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="7e4a3-124">Po Uruchom ten skrypt, albo nie uruchamia animacji jest jeden z dwóch lub czy obu.</span><span class="sxs-lookup"><span data-stu-id="7e4a3-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="7e4a3-125">[![Panel jest wygaszanie bez zmiany rozmiaru, więc drugi działa animacji, pierwsza z nich nie](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7e4a3-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="7e4a3-126">Panel jest wygaszanie bez zmiany rozmiaru, więc drugi działa animacji, pierwsza z nich nie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animation-depending-on-a-condition-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7e4a3-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7e4a3-127">[Poprzednie](executing-several-animations-after-each-other-cs.md)
> [dalej](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7e4a3-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
