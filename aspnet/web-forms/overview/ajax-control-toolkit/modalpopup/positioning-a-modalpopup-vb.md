---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Pozycjonowanie kontrolki ModalPopup (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta. Jednak formant nie oferuje...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1578a1bd6d5e3b595eba526552b8723daefb2ba1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387835"
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="c709c-104">Pozycjonowanie kontrolki ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="c709c-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="c709c-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c709c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c709c-106">[Pobierz program Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c709c-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="c709c-107">Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c709c-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c709c-108">Jednak formant nie oferuje wbudowane funkcje, aby ustalić położenie wyskakującego okienka.</span><span class="sxs-lookup"><span data-stu-id="c709c-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="c709c-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c709c-109">Overview</span></span>

<span data-ttu-id="c709c-110">Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c709c-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c709c-111">Jednak formant nie oferuje wbudowane funkcje, aby ustalić położenie wyskakującego okienka.</span><span class="sxs-lookup"><span data-stu-id="c709c-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="c709c-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="c709c-112">Steps</span></span>

<span data-ttu-id="c709c-113">Aby aktywować tę funkcję ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="c709c-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="c709c-114">Kontrolka musi znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):</span><span class="sxs-lookup"><span data-stu-id="c709c-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="c709c-115">Następnie dodaj panel, który służy jako modalnego okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="c709c-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="c709c-116">Przycisk jest używany, aby zamknąć okno podręczne:</span><span class="sxs-lookup"><span data-stu-id="c709c-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="c709c-117">Zawsze, gdy zostanie wyświetlone okno podręczne, powinien zostać umieszczony w określone miejsce na stronie.</span><span class="sxs-lookup"><span data-stu-id="c709c-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="c709c-118">Dla tego zadania zostanie utworzona funkcja języka JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c709c-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="c709c-119">Najpierw próbuje uzyskać dostęp w panelu.</span><span class="sxs-lookup"><span data-stu-id="c709c-119">It first tries to access the panel.</span></span> <span data-ttu-id="c709c-120">Jeśli się powiedzie, położenie panelu można ustawić przy użyciu CSS i JavaScript (zmiana położenia okna podręcznego na będą).</span><span class="sxs-lookup"><span data-stu-id="c709c-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="c709c-121">Jednak `ModalPopupExtender` kontroli próbuje również położenie wyskakującego okienka.</span><span class="sxs-lookup"><span data-stu-id="c709c-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="c709c-122">W związku z tym kod JavaScript wielokrotnie umieszcza okno podręczne, co dziesiątej sekundy.</span><span class="sxs-lookup"><span data-stu-id="c709c-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="c709c-123">Jak widać, wartość zwracana przez `setTimeout()` metody JavaScript są zapisywane w zmiennej globalnej.</span><span class="sxs-lookup"><span data-stu-id="c709c-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="c709c-124">Dzięki temu można zatrzymać powtarzanych położenia okna podręcznego na żądanie, używając `clearTimeout()` metody:</span><span class="sxs-lookup"><span data-stu-id="c709c-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="c709c-125">Teraz pozostało celu jest zapewnienie przeglądarki wywołać te funkcje, zawsze, gdy jest to odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="c709c-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="c709c-126">`movePanel()` Musi zostać wywołana funkcja języka JavaScript, po kliknięciu przycisku wyzwala panelu:</span><span class="sxs-lookup"><span data-stu-id="c709c-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="c709c-127">I `stopMoving()` funkcja właśnie po zamknięciu okna podręcznego może to nastąpić w `ModalPopupExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="c709c-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="c709c-128">[![Modalnego okna podręcznego, który pojawia się w wyznaczonym miejscu](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c709c-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="c709c-129">Modalnego okna podręcznego, który pojawia się w wyznaczonym miejscu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c709c-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c709c-130">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="c709c-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
