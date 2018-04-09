---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Dopasowywanie indeks cień (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Jednak ta tle czasami powoduje konflikt z inne formanty, zainstaluj...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="12328-104">Dopasowywanie indeks cień (VB)</span><span class="sxs-lookup"><span data-stu-id="12328-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="12328-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="12328-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="12328-106">[Pobierz kod](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="12328-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="12328-107">Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień.</span><span class="sxs-lookup"><span data-stu-id="12328-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="12328-108">Jednak ta tle czasami powoduje konflikt z inne formanty, na przykład kontrolka Menu programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="12328-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="12328-109">Gdy pozycja menu pojawia się, prawdopodobnie za cień.</span><span class="sxs-lookup"><span data-stu-id="12328-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="12328-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="12328-110">Overview</span></span>

<span data-ttu-id="12328-111">Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień.</span><span class="sxs-lookup"><span data-stu-id="12328-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="12328-112">Jednak ta tle czasami powoduje konflikt z inne formanty, na przykład kontrolka Menu programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="12328-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="12328-113">Gdy pozycja menu pojawia się, prawdopodobnie za cień.</span><span class="sxs-lookup"><span data-stu-id="12328-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="12328-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="12328-114">Steps</span></span>

<span data-ttu-id="12328-115">Kod rozpocznie się z panelem, zawierające wystarczającej ilości tekstu, dzięki czemu panelu zawiera za mało tekst dla efektu, które mają być wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="12328-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="12328-116">Inny panel znajduje się bezpośrednio przed `panelShadow` panelu.</span><span class="sxs-lookup"><span data-stu-id="12328-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="12328-117">Zawiera menu orientacji poziomej tak, aby elementy menu są wyświetlane nad (lub zamiast: w obszarze) `dropShadow` panelu):</span><span class="sxs-lookup"><span data-stu-id="12328-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="12328-118">Następnie `DropShadowExtender` jest dodawany do rozszerzania `panelShadow` panel z efektem cienia:</span><span class="sxs-lookup"><span data-stu-id="12328-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="12328-119">Na koniec ASP.NET AJAX `ScriptManager` formant umożliwia kontroli zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="12328-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="12328-120">Po uruchomieniu tego skryptu, elementy menu są wyświetlane poniżej panelu.</span><span class="sxs-lookup"><span data-stu-id="12328-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="12328-121">Jednak menu używa klasy CSS `panel` gdzie wystarczy zdefiniować dwie czynności, aby elementy pojawiają się przed innymi panelu:</span><span class="sxs-lookup"><span data-stu-id="12328-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="12328-122">Pozycjonowanie względne</span><span class="sxs-lookup"><span data-stu-id="12328-122">Relative positioning</span></span>
- <span data-ttu-id="12328-123">Dodatnia indeks</span><span class="sxs-lookup"><span data-stu-id="12328-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="12328-124">Następnie `DropShadowExtender` formantu nie powoduje konfliktu już z Menu kontroli.</span><span class="sxs-lookup"><span data-stu-id="12328-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="12328-125">[![Przed: Wpisu menu nie jest widoczny](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="12328-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="12328-126">Przed: Nie jest widoczna pozycja menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="12328-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="12328-127">[![Po: Pojawi się wpis menu](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="12328-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="12328-128">Po: Pojawi się wpis menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="12328-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="12328-129">[Poprzednie](manipulating-dropshadow-properties-from-client-code-cs.md)
> [dalej](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="12328-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
