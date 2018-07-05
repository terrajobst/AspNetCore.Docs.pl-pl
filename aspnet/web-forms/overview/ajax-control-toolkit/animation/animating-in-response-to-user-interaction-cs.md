---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Wykonywanie animacji w odpowiedzi na interakcję z użytkownikiem (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można w formie gwiazdek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 9dea9daf3df76558eb19a524475cedd8e2085297
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379822"
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="e126e-104">Wykonywanie animacji w odpowiedzi na interakcję z użytkownikiem (C#)</span><span class="sxs-lookup"><span data-stu-id="e126e-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="e126e-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e126e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e126e-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e126e-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="e126e-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="e126e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e126e-108">Animacji można uruchomić automatycznie lub mogą być powodowane przez interakcję użytkownika, np. przez kliknięcie myszą.</span><span class="sxs-lookup"><span data-stu-id="e126e-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="e126e-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="e126e-109">Overview</span></span>

<span data-ttu-id="e126e-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="e126e-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e126e-111">Animacji można uruchomić automatycznie lub mogą być powodowane przez interakcję użytkownika, np. przez kliknięcie myszą.</span><span class="sxs-lookup"><span data-stu-id="e126e-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="e126e-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="e126e-112">Steps</span></span>

<span data-ttu-id="e126e-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="e126e-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="e126e-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e126e-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="e126e-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="e126e-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="e126e-116">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="e126e-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="e126e-117">W ramach `<Animations>` węzła, istnieje pięć sposobów rozpoczęcia animacji za pośrednictwem interakcji z użytkownikiem (Brak elementu jest `<OnLoad>` które jest wykonywane po całkowitym załadowaniu całej strony):</span><span class="sxs-lookup"><span data-stu-id="e126e-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="e126e-118">`<OnClick>` (kliknięcie myszą kontrolki)</span><span class="sxs-lookup"><span data-stu-id="e126e-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="e126e-119">`<OnHoverOut>` (kursora myszy poza formant)</span><span class="sxs-lookup"><span data-stu-id="e126e-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="e126e-120">`<OnHoverOver>` (wskaźnika myszy nad formant zatrzymywanie `<OnHoverOut>` animacji)</span><span class="sxs-lookup"><span data-stu-id="e126e-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="e126e-121">`<OnMouseOut>` (kursora myszy poza formant)</span><span class="sxs-lookup"><span data-stu-id="e126e-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="e126e-122">`<OnMouseOver>` (wskaźnika myszy nad formant nie zatrzymywanie `<OnMouseOut>` animacji)</span><span class="sxs-lookup"><span data-stu-id="e126e-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="e126e-123">W tym scenariuszu `<OnClick>` jest używany.</span><span class="sxs-lookup"><span data-stu-id="e126e-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="e126e-124">Gdy użytkownik kliknie na panelu, rozmiaru i stopniowo zmniejsza się w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="e126e-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="e126e-125">[![Kliknięcie myszą powoduje uruchomienie animacji](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e126e-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="e126e-126">Kliknięcie myszą powoduje uruchomienie animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e126e-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e126e-127">[Poprzednie](picking-one-animation-out-of-a-list-cs.md)
> [dalej](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e126e-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
