---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: "Dopasowywanie indeks cień (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Jednak ta tle czasami powoduje konflikt z inne formanty, zainstaluj..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 161f02daa5dd1f0e21853c1b7c1a65c1a9aa5d03
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="2d814-104">Dopasowywanie indeks cień (C#)</span><span class="sxs-lookup"><span data-stu-id="2d814-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>
====================
<span data-ttu-id="2d814-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2d814-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2d814-106">[Pobierz kod](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2d814-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="2d814-107">Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień.</span><span class="sxs-lookup"><span data-stu-id="2d814-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="2d814-108">Jednak ta tle czasami powoduje konflikt z inne formanty, na przykład kontrolka Menu programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2d814-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="2d814-109">Gdy pozycja menu pojawia się, prawdopodobnie za cień.</span><span class="sxs-lookup"><span data-stu-id="2d814-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="2d814-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2d814-110">Overview</span></span>

<span data-ttu-id="2d814-111">Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień.</span><span class="sxs-lookup"><span data-stu-id="2d814-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="2d814-112">Jednak ta tle czasami powoduje konflikt z inne formanty, na przykład kontrolka Menu programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2d814-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="2d814-113">Gdy pozycja menu pojawia się, prawdopodobnie za cień.</span><span class="sxs-lookup"><span data-stu-id="2d814-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="2d814-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="2d814-114">Steps</span></span>

<span data-ttu-id="2d814-115">Kod rozpocznie się z panelem, zawierające wystarczającej ilości tekstu, dzięki czemu panelu zawiera za mało tekst dla efektu, które mają być wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="2d814-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="2d814-116">Inny panel znajduje się bezpośrednio przed `panelShadow` panelu.</span><span class="sxs-lookup"><span data-stu-id="2d814-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="2d814-117">Zawiera menu orientacji poziomej tak, aby elementy menu są wyświetlane nad (lub zamiast: w obszarze) `dropShadow` panelu):</span><span class="sxs-lookup"><span data-stu-id="2d814-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="2d814-118">Następnie `DropShadowExtender` jest dodawany do rozszerzania `panelShadow` panel z efektem cienia:</span><span class="sxs-lookup"><span data-stu-id="2d814-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="2d814-119">Na koniec ASP.NET AJAX `ScriptManager` formant umożliwia kontroli zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="2d814-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="2d814-120">Po uruchomieniu tego skryptu, elementy menu są wyświetlane poniżej panelu.</span><span class="sxs-lookup"><span data-stu-id="2d814-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="2d814-121">Jednak menu używa klasy CSS `panel` gdzie wystarczy zdefiniować dwie czynności, aby elementy pojawiają się przed innymi panelu:</span><span class="sxs-lookup"><span data-stu-id="2d814-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="2d814-122">Pozycjonowanie względne</span><span class="sxs-lookup"><span data-stu-id="2d814-122">Relative positioning</span></span>
- <span data-ttu-id="2d814-123">Dodatnia indeks</span><span class="sxs-lookup"><span data-stu-id="2d814-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="2d814-124">Następnie `DropShadowExtender` formantu nie powoduje konfliktu już z Menu kontroli.</span><span class="sxs-lookup"><span data-stu-id="2d814-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="2d814-125">[![Przed: Wpisu menu nie jest widoczny](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2d814-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="2d814-126">Przed: Nie jest widoczna pozycja menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2d814-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>


<span data-ttu-id="2d814-127">[![Po: Pojawi się wpis menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2d814-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="2d814-128">Po: Pojawi się wpis menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2d814-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="2d814-129">Dalej</span><span class="sxs-lookup"><span data-stu-id="2d814-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
