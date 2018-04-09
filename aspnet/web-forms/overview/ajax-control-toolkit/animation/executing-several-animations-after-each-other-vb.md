---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Wykonywanie kilku animacji po sobie nawzajem (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Umożliwia uruchamianie severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 700946b9f32c5ed2dcb8586e7c0e84d2238ff103
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="04607-104">Wykonywanie kilku animacji po sobie nawzajem (VB)</span><span class="sxs-lookup"><span data-stu-id="04607-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="04607-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="04607-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="04607-106">[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="04607-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="04607-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="04607-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="04607-108">Umożliwia uruchamianie kilka animacji jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="04607-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="04607-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="04607-109">Overview</span></span>

<span data-ttu-id="04607-110">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="04607-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="04607-111">Umożliwia uruchamianie kilka animacji jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="04607-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="04607-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="04607-112">Steps</span></span>

<span data-ttu-id="04607-113">Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="04607-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="04607-114">Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="04607-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="04607-115">Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="04607-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="04607-116">Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="04607-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="04607-117">W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchomienia animacji po całkowitym załadowaniem strony.</span><span class="sxs-lookup"><span data-stu-id="04607-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="04607-118">Ogólnie rzecz biorąc `<OnLoad>` akceptuje tylko jednej animacji.</span><span class="sxs-lookup"><span data-stu-id="04607-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="04607-119">Framework animacji można sprzęgać kilka animacji w jednej przy użyciu `<Sequence>` elementu.</span><span class="sxs-lookup"><span data-stu-id="04607-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="04607-120">Wszystkie animacje w `<Sequence>` są wykonywane jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="04607-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="04607-121">Oto możliwe kod znaczników dla `AnimationExtender` kontroli, najpierw udostępniania panelu szersze i następnie zmniejszenie jego wysokość:</span><span class="sxs-lookup"><span data-stu-id="04607-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="04607-122">Po uruchomieniu tego skryptu panelu otrzymuje szersze, a następnie mniejszy.</span><span class="sxs-lookup"><span data-stu-id="04607-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="04607-123">[![Najpierw zwiększa się szerokość](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="04607-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="04607-124">Najpierw zwiększa się szerokość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="04607-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="04607-125">[![Następnie zostaje zmniejszona wysokość](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="04607-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="04607-126">A następnie zostaje zmniejszona wysokość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="04607-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="04607-127">[Poprzednie](executing-several-animations-at-the-same-time-vb.md)
> [dalej](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="04607-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
