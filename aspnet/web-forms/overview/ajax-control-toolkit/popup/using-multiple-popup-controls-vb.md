---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Używanie wielu formantów podręcznego (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Istnieje również możliwość użycia m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c57aab3ecf2c02a8488b5ea4e3e0ed33ac5e7fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869325"
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="35b3a-104">Używanie wielu formantów podręcznego (VB)</span><span class="sxs-lookup"><span data-stu-id="35b3a-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="35b3a-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="35b3a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="35b3a-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="35b3a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="35b3a-107">Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="35b3a-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="35b3a-108">Istnieje również możliwość używania więcej niż jeden formant menu podręczne na jednej stronie.</span><span class="sxs-lookup"><span data-stu-id="35b3a-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="35b3a-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="35b3a-109">Overview</span></span>

<span data-ttu-id="35b3a-110">Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="35b3a-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="35b3a-111">Istnieje również możliwość używania więcej niż jeden formant menu podręczne na jednej stronie.</span><span class="sxs-lookup"><span data-stu-id="35b3a-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="35b3a-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="35b3a-112">Steps</span></span>

<span data-ttu-id="35b3a-113">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="35b3a-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="35b3a-114">Następnie dodaj panel, która służy jako menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="35b3a-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="35b3a-115">W tym scenariuszu zawiera panelu `Calendar` formantu.</span><span class="sxs-lookup"><span data-stu-id="35b3a-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="35b3a-116">Aby uniknąć odświeżenie strony spowodowane ogłaszania zwrotnego kalendarza, panelu jest umieszczany w `UpdatePanel` sterowania:</span><span class="sxs-lookup"><span data-stu-id="35b3a-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="35b3a-117">Strona zawiera również dwóch pól tekstowych.</span><span class="sxs-lookup"><span data-stu-id="35b3a-117">The page also contains two text boxes.</span></span> <span data-ttu-id="35b3a-118">Dla każdego pola tekstowego kalendarza podręcznego są wyświetlane po uaktywnieniu pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="35b3a-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="35b3a-119">Teraz Rozszerz każdą z dwóch polach z `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="35b3a-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="35b3a-120">`TargetControlID` Atrybutu zawiera identyfikator formantu związana z rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="35b3a-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="35b3a-121">`PopupControlID` Atrybutu zawiera identyfikator panel menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="35b3a-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="35b3a-122">W takim przypadku zarówno Extender Pokaż samym panelu, ale panele różne możliwe, są również.</span><span class="sxs-lookup"><span data-stu-id="35b3a-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="35b3a-123">Teraz przy każdym kliknięciu w polu tekstowym, kalendarz pojawia się poniżej pola, co pozwala na wybranie daty.</span><span class="sxs-lookup"><span data-stu-id="35b3a-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="35b3a-124">(Odzyskać wybranej daty w polach tekstowych zostanie omówiona w samouczku różnych.)</span><span class="sxs-lookup"><span data-stu-id="35b3a-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="35b3a-125">[![Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="35b3a-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="35b3a-126">Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="35b3a-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="35b3a-127">[Poprzednie](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [dalej](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="35b3a-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
