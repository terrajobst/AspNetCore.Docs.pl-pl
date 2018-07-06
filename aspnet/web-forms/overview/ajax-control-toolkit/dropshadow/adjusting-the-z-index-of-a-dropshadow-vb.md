---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Dostosowywanie indeksu z kontrolki DropShadow (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia. Jednak to w tle czasami powoduje konflikt z innych formantów, aby uzyskać Zainstaluj...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 78697f51a09dfaad315255efa23120d4c456bfea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829547"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="7c024-104">Dostosowywanie indeksu z kontrolki DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="7c024-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="7c024-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7c024-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7c024-106">[Pobierz program Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7c024-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="7c024-107">Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia.</span><span class="sxs-lookup"><span data-stu-id="7c024-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="7c024-108">Jednak to w tle czasami jest w konflikcie z innymi formantami, na przykład formant Menu platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7c024-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="7c024-109">Gdy wpis menu pojawia się, prawdopodobnie za cienia.</span><span class="sxs-lookup"><span data-stu-id="7c024-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="7c024-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="7c024-110">Overview</span></span>

<span data-ttu-id="7c024-111">Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia.</span><span class="sxs-lookup"><span data-stu-id="7c024-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="7c024-112">Jednak to w tle czasami jest w konflikcie z innymi formantami, na przykład formant Menu platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7c024-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="7c024-113">Gdy wpis menu pojawia się, prawdopodobnie za cienia.</span><span class="sxs-lookup"><span data-stu-id="7c024-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="7c024-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="7c024-114">Steps</span></span>

<span data-ttu-id="7c024-115">Kod rozpocznie się za pomocą panelu, zawierające wystarczającej ilości tekstu, dzięki czemu na panelu zawiera wystarczająco dużo tekstu dla efektu były widoczne:</span><span class="sxs-lookup"><span data-stu-id="7c024-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="7c024-116">Kolejny panel jest umieszczane bezpośrednio przed `panelShadow` panelu.</span><span class="sxs-lookup"><span data-stu-id="7c024-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="7c024-117">Zawiera menu w orientacji poziomej, tak, aby elementy menu są wyświetlane nad (lub zamiast: w obszarze) `dropShadow` panelu):</span><span class="sxs-lookup"><span data-stu-id="7c024-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="7c024-118">Następnie `DropShadowExtender` jest dodawany do rozszerzenia `panelShadow` panelu z efektem cienia:</span><span class="sxs-lookup"><span data-stu-id="7c024-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="7c024-119">Na koniec ASP.NET AJAX `ScriptManager` control umożliwia kontrolki zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="7c024-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="7c024-120">Po uruchomieniu tego skryptu, elementy menu wyświetlane w obszarze panelu.</span><span class="sxs-lookup"><span data-stu-id="7c024-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="7c024-121">Jednak menu używa klasy CSS `panel` gdzie wystarczy zdefiniować dwie czynności w celu zapewnienia elementy są wyświetlane przed innymi panelu:</span><span class="sxs-lookup"><span data-stu-id="7c024-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="7c024-122">Pozycjonowanie względne</span><span class="sxs-lookup"><span data-stu-id="7c024-122">Relative positioning</span></span>
- <span data-ttu-id="7c024-123">Dodatnia indeks</span><span class="sxs-lookup"><span data-stu-id="7c024-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="7c024-124">Następnie `DropShadowExtender` formantu nie powoduje już konfliktu z formant Menu.</span><span class="sxs-lookup"><span data-stu-id="7c024-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="7c024-125">[![Przed: Wpis menu nie jest widoczny](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7c024-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="7c024-126">Przed: Wpis menu nie jest widoczny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7c024-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="7c024-127">[![Po: Pojawi się wpis menu](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="7c024-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="7c024-128">Po: Pojawi się wpis menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7c024-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7c024-129">[Poprzednie](manipulating-dropshadow-properties-from-client-code-cs.md)
> [dalej](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7c024-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
