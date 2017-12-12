---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Pozycjonowanie ModalPopup (VB) | Dokumentacja firmy Microsoft
author: wenz
description: "Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Jednak formantu nie oferuje..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b9c0790d1696bcf478bcdea089d4d3d92450369
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="82282-104">Pozycjonowanie ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="82282-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="82282-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="82282-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="82282-106">[Pobierz kod](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="82282-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="82282-107">Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="82282-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="82282-108">Formant nie oferuje jednak wbudowanej funkcji do pozycji menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="82282-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="82282-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="82282-109">Overview</span></span>

<span data-ttu-id="82282-110">Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="82282-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="82282-111">Formant nie oferuje jednak wbudowanej funkcji do pozycji menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="82282-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="82282-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="82282-112">Steps</span></span>

<span data-ttu-id="82282-113">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="82282-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="82282-114">formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="82282-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="82282-115">Następnie dodaj panel, która służy jako modalnych menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="82282-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="82282-116">Przycisk służy do zamykania menu podręcznego:</span><span class="sxs-lookup"><span data-stu-id="82282-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="82282-117">Zawsze, gdy podręczny jest wyświetlany, powinien zostać umieszczony w określone miejsce na stronie.</span><span class="sxs-lookup"><span data-stu-id="82282-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="82282-118">Dla tego zadania jest tworzony funkcji JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="82282-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="82282-119">Najpierw próbuje uzyskać dostęp z panelu.</span><span class="sxs-lookup"><span data-stu-id="82282-119">It first tries to access the panel.</span></span> <span data-ttu-id="82282-120">Jeśli zakończy się powodzeniem, pozycji panel jest ustawiany za pomocą CSS i JavaScript (zmiana spowoduje pozycja podręcznego w).</span><span class="sxs-lookup"><span data-stu-id="82282-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="82282-121">Jednak `ModalPopupExtender` kontroli próbuje również pozycji menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="82282-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="82282-122">W związku z tym kod JavaScript wielokrotnie umieszcza menu podręczne, co dziesiątym dniu sekundy.</span><span class="sxs-lookup"><span data-stu-id="82282-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="82282-123">Jak widać, zwracana wartość `setTimeout()` metody JavaScript jest zapisywany w zmiennej globalnej.</span><span class="sxs-lookup"><span data-stu-id="82282-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="82282-124">Dzięki temu można zatrzymać powtarzane pozycjonowanie podręcznego na żądanie, używając `clearTimeout()` metody:</span><span class="sxs-lookup"><span data-stu-id="82282-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="82282-125">Teraz pozostało celu jest ustawienie przeglądarki wywoływać te funkcje, w miarę potrzeb.</span><span class="sxs-lookup"><span data-stu-id="82282-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="82282-126">`movePanel()` Można wywołać funkcji JavaScript, po kliknięciu przycisku wyzwala panelu:</span><span class="sxs-lookup"><span data-stu-id="82282-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="82282-127">I `stopMoving()` funkcja wejścia play po zamknięciu menu podręcznego. może to być wyzwalane w `ModalPopupExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="82282-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="82282-128">[![W pozycji wyznaczonych pojawi się modalne okno podręczne](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="82282-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="82282-129">Modalne wyskakującego na wyznaczonych pozycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="82282-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="82282-130">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="82282-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
