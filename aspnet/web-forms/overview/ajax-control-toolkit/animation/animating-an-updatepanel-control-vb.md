---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animowanie kontrolki UpdatePanel (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Dla zawartości...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c508d549c7740079b02da323c655c9f789cfd3b9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805304"
---
<a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="9bbb5-104">Animowanie kontrolki UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="9bbb5-104">Animating an UpdatePanel Control (VB)</span></span>
====================
<span data-ttu-id="9bbb5-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9bbb5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9bbb5-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9bbb5-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="9bbb5-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9bbb5-108">Dla zawartości UpdatePanel specjalne urządzenia extender istnieje silnie zależy od ram animacji: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="9bbb5-109">W tym samouczku pokazano, jak skonfigurować takie animacji dla kontrolki UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="9bbb5-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="9bbb5-110">Overview</span></span>

<span data-ttu-id="9bbb5-111">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9bbb5-112">Dla zawartości `UpdatePanel`, specjalne urządzenia extender istnieje która intensywnie korzysta z framework animacji: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="9bbb5-113">W tym samouczku pokazano, jak skonfigurować animacji dla `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="9bbb5-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="9bbb5-114">Steps</span></span>

<span data-ttu-id="9bbb5-115">Pierwszym krokiem jest jak zwykle obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX jest ładowany i można go używać razem sterowania:</span><span class="sxs-lookup"><span data-stu-id="9bbb5-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="9bbb5-116">Animacja w tym scenariuszu zostaną zastosowane do programu ASP.NET `Wizard` formantu sieci web znajdującej się w `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="9bbb5-117">Trzy kroki (dowolną) zawierają wystarczającej liczby opcji, aby wyzwolić ogłaszania zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="9bbb5-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="9bbb5-118">Znaczniki, które są niezbędne do `UpdatePanelAnimationExtender` kontroli jest podobna do znaczników używany dla `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="9bbb5-119">W `TargetControlID` atrybutu, firma Microsoft zapewnia `ID` z `UpdatePanel` animować; w ramach `UpdatePanelAnimationExtender` kontroli `<Animations>` element posiada znaczników XML dla animation(s).</span><span class="sxs-lookup"><span data-stu-id="9bbb5-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="9bbb5-120">Jednak jest jedną różnicą: ilość zdarzenia i procedury obsługi zdarzeń jest ograniczona w porównaniu z `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="9bbb5-121">Aby uzyskać `UpdatePanels`, tylko dwa z nich istnieje:</span><span class="sxs-lookup"><span data-stu-id="9bbb5-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="9bbb5-122">`<OnUpdated>` Po zaktualizowaniu kontrolki UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="9bbb5-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="9bbb5-123">`<OnUpdating>` Po rozpoczęciu aktualizowania kontrolki UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="9bbb5-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="9bbb5-124">W tym scenariuszu nowej zawartości `UpdatePanel` (po zwrotu) są zanikanie.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="9bbb5-125">Jest to niezbędne znaczników do tego:</span><span class="sxs-lookup"><span data-stu-id="9bbb5-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="9bbb5-126">Teraz przy każdym wystąpieniu ogłaszania zwrotnego w ramach kontrolki UpdatePanel, nowej zawartości panelu zanikanie.</span><span class="sxs-lookup"><span data-stu-id="9bbb5-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="9bbb5-127">[![Stopniowe następnego kroku w Kreatorze](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9bbb5-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="9bbb5-128">Stopniowe następnego kroku w Kreatorze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9bbb5-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9bbb5-129">[Poprzednie](changing-an-animation-using-client-side-code-vb.md)
> [dalej](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9bbb5-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
