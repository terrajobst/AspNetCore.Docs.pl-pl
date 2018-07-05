---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Dostosowywanie indeksu z kontrolki DropShadow (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia. Jednak to w tle czasami powoduje konflikt z innych formantów, aby uzyskać Zainstaluj...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 22732bc44a56819a2ec73346dc32659d027eaee0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369573"
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="b1214-104">Dostosowywanie indeksu z kontrolki DropShadow (C#)</span><span class="sxs-lookup"><span data-stu-id="b1214-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>
====================
<span data-ttu-id="b1214-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b1214-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b1214-106">[Pobierz program Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b1214-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="b1214-107">Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia.</span><span class="sxs-lookup"><span data-stu-id="b1214-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="b1214-108">Jednak to w tle czasami jest w konflikcie z innymi formantami, na przykład formant Menu platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b1214-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="b1214-109">Gdy wpis menu pojawia się, prawdopodobnie za cienia.</span><span class="sxs-lookup"><span data-stu-id="b1214-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="b1214-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b1214-110">Overview</span></span>

<span data-ttu-id="b1214-111">Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia.</span><span class="sxs-lookup"><span data-stu-id="b1214-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="b1214-112">Jednak to w tle czasami jest w konflikcie z innymi formantami, na przykład formant Menu platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b1214-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="b1214-113">Gdy wpis menu pojawia się, prawdopodobnie za cienia.</span><span class="sxs-lookup"><span data-stu-id="b1214-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="b1214-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="b1214-114">Steps</span></span>

<span data-ttu-id="b1214-115">Kod rozpocznie się za pomocą panelu, zawierające wystarczającej ilości tekstu, dzięki czemu na panelu zawiera wystarczająco dużo tekstu dla efektu były widoczne:</span><span class="sxs-lookup"><span data-stu-id="b1214-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="b1214-116">Kolejny panel jest umieszczane bezpośrednio przed `panelShadow` panelu.</span><span class="sxs-lookup"><span data-stu-id="b1214-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="b1214-117">Zawiera menu w orientacji poziomej, tak, aby elementy menu są wyświetlane nad (lub zamiast: w obszarze) `dropShadow` panelu):</span><span class="sxs-lookup"><span data-stu-id="b1214-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="b1214-118">Następnie `DropShadowExtender` jest dodawany do rozszerzenia `panelShadow` panelu z efektem cienia:</span><span class="sxs-lookup"><span data-stu-id="b1214-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="b1214-119">Na koniec ASP.NET AJAX `ScriptManager` control umożliwia kontrolki zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="b1214-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="b1214-120">Po uruchomieniu tego skryptu, elementy menu wyświetlane w obszarze panelu.</span><span class="sxs-lookup"><span data-stu-id="b1214-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="b1214-121">Jednak menu używa klasy CSS `panel` gdzie wystarczy zdefiniować dwie czynności w celu zapewnienia elementy są wyświetlane przed innymi panelu:</span><span class="sxs-lookup"><span data-stu-id="b1214-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="b1214-122">Pozycjonowanie względne</span><span class="sxs-lookup"><span data-stu-id="b1214-122">Relative positioning</span></span>
- <span data-ttu-id="b1214-123">Dodatnia indeks</span><span class="sxs-lookup"><span data-stu-id="b1214-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="b1214-124">Następnie `DropShadowExtender` formantu nie powoduje już konfliktu z formant Menu.</span><span class="sxs-lookup"><span data-stu-id="b1214-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="b1214-125">[![Przed: Wpis menu nie jest widoczny](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b1214-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="b1214-126">Przed: Wpis menu nie jest widoczny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b1214-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>


<span data-ttu-id="b1214-127">[![Po: Pojawi się wpis menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="b1214-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="b1214-128">Po: Pojawi się wpis menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b1214-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b1214-129">Next</span><span class="sxs-lookup"><span data-stu-id="b1214-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
