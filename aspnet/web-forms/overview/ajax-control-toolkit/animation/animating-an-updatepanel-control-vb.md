---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animowanie formantu UpdatePanel (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Zawartości...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c1114b74fd152a4ea85aa10850860f75573adee
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873160"
---
<a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="51931-104">Animowanie formantu UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="51931-104">Animating an UpdatePanel Control (VB)</span></span>
====================
<span data-ttu-id="51931-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="51931-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="51931-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="51931-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="51931-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="51931-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="51931-108">Zawartość elementu UpdatePanel, rozszerzający specjalne umożliwiającą odgrywa framework animacji: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="51931-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="51931-109">Ten samouczek pokazuje, jak skonfigurować takie animacji dla elementu UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="51931-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="51931-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="51931-110">Overview</span></span>

<span data-ttu-id="51931-111">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="51931-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="51931-112">Zawartości `UpdatePanel`, rozszerzający specjalne umożliwiającą odgrywa framework animacji: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="51931-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="51931-113">W tym samouczku przedstawiono sposób ustawiania animacji dla `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="51931-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="51931-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="51931-114">Steps</span></span>

<span data-ttu-id="51931-115">Pierwszym krokiem jest normalnie obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX została załadowana i można go używać zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="51931-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="51931-116">Animacja w tym scenariuszu zostaną zastosowane dla platformy ASP.NET `Wizard` kontrolka sieci web znajdującej się w `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="51931-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="51931-117">Trzy kroki (dowolną) umożliwiają za mało wyzwolenia ogłaszania zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="51931-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="51931-118">Kod znaczników, które są niezbędne do `UpdatePanelAnimationExtender` formant jest podobna do znacznika używany do `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="51931-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="51931-119">W `TargetControlID` atrybutu udostępniamy `ID` z `UpdatePanel` do animowania; w `UpdatePanelAnimationExtender` kontroli, `<Animations>` element posiada znaczników XML dla animacje.</span><span class="sxs-lookup"><span data-stu-id="51931-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="51931-120">Jednak nie ma różnicy jednego: ilość zdarzenia i procedury obsługi zdarzeń jest ograniczona w porównaniu z `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="51931-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="51931-121">Aby uzyskać `UpdatePanels`, tylko dwa z nich istnieje:</span><span class="sxs-lookup"><span data-stu-id="51931-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="51931-122">`<OnUpdated>` Po zaktualizowaniu UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="51931-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="51931-123">`<OnUpdating>` Po uruchomieniu aktualizacji UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="51931-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="51931-124">W tym scenariuszu nowej zawartości `UpdatePanel` (po odświeżenie strony) są zanikania.</span><span class="sxs-lookup"><span data-stu-id="51931-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="51931-125">Jest to niezbędne znacznika, w tym:</span><span class="sxs-lookup"><span data-stu-id="51931-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="51931-126">Teraz po zmianie odświeżania strony w elemencie UpdatePanel, nowej zawartości panelu zanikania sprawnie.</span><span class="sxs-lookup"><span data-stu-id="51931-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="51931-127">[![Następny krok kreatora zanikania jest](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="51931-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="51931-128">Następny krok kreatora zanikania jest ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="51931-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="51931-129">[Poprzednie](changing-an-animation-using-client-side-code-vb.md)
> [dalej](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="51931-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
