---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animacja w zależności od warunku (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Czy animacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 160b53519382c215db039fd6297bb907688b81c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368560"
---
<a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="85a15-104">Animacja w zależności od warunku (VB)</span><span class="sxs-lookup"><span data-stu-id="85a15-104">Animation Depending On a Condition (VB)</span></span>
====================
<span data-ttu-id="85a15-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="85a15-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="85a15-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="85a15-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="85a15-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="85a15-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="85a15-108">Czy animacji jest uruchomiony lub nie może także zależeć od warunku w postaci kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="85a15-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="85a15-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="85a15-109">Overview</span></span>

<span data-ttu-id="85a15-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="85a15-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="85a15-111">Czy animacji jest uruchomiony lub nie może także zależeć od warunku w postaci kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="85a15-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="85a15-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="85a15-112">Steps</span></span>

<span data-ttu-id="85a15-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="85a15-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="85a15-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="85a15-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="85a15-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="85a15-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="85a15-116">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="85a15-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="85a15-117">W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchamiania animacji, po całkowitym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="85a15-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="85a15-118">Zamiast jednej z regularnych animacji `<Condition>` element, który jest dostarczany do gry.</span><span class="sxs-lookup"><span data-stu-id="85a15-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="85a15-119">Kod JavaScript, podana jako wartość `ConditionScript` atrybutu jest wykonywane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="85a15-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="85a15-120">Jeśli ma wartość true, animacji jest wykonywana, w przeciwnym razie nie.</span><span class="sxs-lookup"><span data-stu-id="85a15-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="85a15-121">Następujące znaczniki zawiera dwa animacji, każdy z nich jest wykonywana w 50% przypadkach na losowe.</span><span class="sxs-lookup"><span data-stu-id="85a15-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="85a15-122">Ponieważ może istnieć tylko jeden animacji w ramach `<OnLoad>`, dwa `<Condition>` animacji są łączone ze sobą przy użyciu `<Sequence>` elementu:</span><span class="sxs-lookup"><span data-stu-id="85a15-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="85a15-123">Należy pamiętać, że znak mniejszości (`<`) w `ConditionScript` atrybut musi być o zmienionym znaczeniu ().</span><span class="sxs-lookup"><span data-stu-id="85a15-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="85a15-124">Po uruchomieniu tego skryptu, albo brak uruchomień animacji jest jednym z dwóch lub oba są.</span><span class="sxs-lookup"><span data-stu-id="85a15-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="85a15-125">[![Panel jest wygaszanie bez zmiany rozmiaru, więc drugiego uruchomienia animacji, pierwszy z nich nie](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="85a15-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="85a15-126">Panel jest wygaszanie bez zmiany rozmiaru, więc drugiego uruchomienia animacji, pierwszy z nich nie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="85a15-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="85a15-127">[Poprzednie](executing-several-animations-after-each-other-vb.md)
> [dalej](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="85a15-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
