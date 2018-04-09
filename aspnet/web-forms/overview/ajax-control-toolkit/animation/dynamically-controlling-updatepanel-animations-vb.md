---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Dynamicznie kontrolowanie UpdatePanel animacji (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Zawartości...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: ff2853b4457a83a7367b4d1072d21929c40a3ed2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="da9b3-104">Dynamicznie kontrolowanie UpdatePanel animacji (VB)</span><span class="sxs-lookup"><span data-stu-id="da9b3-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>
====================
<span data-ttu-id="da9b3-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="da9b3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="da9b3-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="da9b3-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="da9b3-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="da9b3-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="da9b3-108">Zawartość elementu UpdatePanel, rozszerzający specjalne umożliwiającą odgrywa framework animacji: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="da9b3-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="da9b3-109">Można go także używać razem z wyzwalaczy elementu UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="da9b3-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="da9b3-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="da9b3-110">Overview</span></span>

<span data-ttu-id="da9b3-111">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="da9b3-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="da9b3-112">Zawartości `UpdatePanel`, rozszerzający specjalne umożliwiającą odgrywa framework animacji: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="da9b3-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="da9b3-113">Można go także używać razem z `UpdatePanel` wyzwalaczy.</span><span class="sxs-lookup"><span data-stu-id="da9b3-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="da9b3-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="da9b3-114">Steps</span></span>

<span data-ttu-id="da9b3-115">Pierwszym krokiem jest normalnie obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX została załadowana i można go używać zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="da9b3-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="da9b3-116">Animacja w tym scenariuszu zostaną zastosowane do wyświetlenia bieżącego czasu.</span><span class="sxs-lookup"><span data-stu-id="da9b3-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="da9b3-117">Te informacje mogą być zapisywane w etykiecie przy użyciu `Page_Load()` metody, lub (dla uproszczenia należy) jest używany następujący kod wbudowany:</span><span class="sxs-lookup"><span data-stu-id="da9b3-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="da9b3-118">Ponadto jest tworzony przycisk, aby wyzwolić aktualizowanie czas:</span><span class="sxs-lookup"><span data-stu-id="da9b3-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="da9b3-119">Ten kod jest następnie poddane `<ContentTemplate>` sekcji `UpdatePanel` elementu.</span><span class="sxs-lookup"><span data-stu-id="da9b3-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="da9b3-120">Panelu `UpdateMode` atrybut musi być ustawiony na `"Conditional"`, ponieważ tylko wyzwalaczy może zaktualizować zawartość panelu.</span><span class="sxs-lookup"><span data-stu-id="da9b3-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="da9b3-121">W `<Triggers>` sekcji `UpdatePanel`, wyzwalacz odświeżania strony asynchroniczne jest tworzony i powiązane `Click` zdarzeń przycisku.</span><span class="sxs-lookup"><span data-stu-id="da9b3-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="da9b3-122">W związku z tym, gdy użytkownik kliknie przycisk, `UpdatePanel` zostanie odświeżony.</span><span class="sxs-lookup"><span data-stu-id="da9b3-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="da9b3-123">Oto kod znaczników dla `UpdatePanel` sterowania:</span><span class="sxs-lookup"><span data-stu-id="da9b3-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="da9b3-124">Na koniec `UpdatePanelAnimationExtender` musi być skonfigurowany: Ustaw `TargetControlID` atrybutu ID panelu, a także definiują animacji w rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="da9b3-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="da9b3-125">Zanikania w ułatwia wykrywanie, który tworzy nieuprzywilejowany visual nacisk położono na czas zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="da9b3-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="da9b3-126">Z rozszerzeń znaczników może następnie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="da9b3-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="da9b3-127">Uruchom plik w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="da9b3-127">Run the file in the browser.</span></span> <span data-ttu-id="da9b3-128">Po kliknięciu przycisku bieżący czas jest wyświetlany w panelu zawsze zanikania czasie trwania jednej sekundzie.</span><span class="sxs-lookup"><span data-stu-id="da9b3-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="da9b3-129">[![Bieżący czas jest zanikania](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="da9b3-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="da9b3-130">Bieżący czas jest zanikania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="da9b3-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="da9b3-131">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="da9b3-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
