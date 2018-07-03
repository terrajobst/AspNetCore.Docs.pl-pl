---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Obsługa ogłaszania zwrotnego w kontrolce Popup bez kontrolki UpdatePanel (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Jeśli odświeżenie strony występuje w su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: d74a44a277bdcb460dc20b78bad3e5ed68445b4a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370986"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="21248-104">Obsługa ogłaszania zwrotnego w kontrolce Popup bez kontrolki UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="21248-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="21248-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="21248-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="21248-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="21248-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="21248-107">Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami.</span><span class="sxs-lookup"><span data-stu-id="21248-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="21248-108">Podczas ogłaszania zwrotnego odbywa się w taki zespół wiąże się z kilku panelach na stronie trudno jest określić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="21248-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="21248-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="21248-109">Overview</span></span>

<span data-ttu-id="21248-110">Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami.</span><span class="sxs-lookup"><span data-stu-id="21248-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="21248-111">Podczas ogłaszania zwrotnego odbywa się w taki zespół wiąże się z kilku panelach na stronie trudno jest określić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="21248-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="21248-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="21248-112">Steps</span></span>

<span data-ttu-id="21248-113">Korzystając z `PopupControl` odświeżenie strony, ale bez konieczności `UpdatePanel` na stronie Toolkit formantu nie oferuje możliwość określenia, który element klienta wyzwolił okna podręcznego, który z kolei spowodował zwrotu.</span><span class="sxs-lookup"><span data-stu-id="21248-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="21248-114">Jednak małych lewy dostarcza obejście dla tego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="21248-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="21248-115">Po pierwsze, w tym miejscu jest konfiguracja podstawowa: dwóch pól tekstowych, które wyzwolić tego samego okna podręcznego kalendarza.</span><span class="sxs-lookup"><span data-stu-id="21248-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="21248-116">Dwa `PopupControlExtenders` łączą ze sobą pola tekstowe i menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="21248-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="21248-117">Podstawowa koncepcja jest dodanie ukryte pole formularza w &lt; `form` &gt; element, który posiada pola tekstowego, która została uruchomiona, okno podręczne:</span><span class="sxs-lookup"><span data-stu-id="21248-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="21248-118">Po załadowaniu strony kodu w języku JavaScript dodaje procedurę obsługi zdarzeń do obu polach tekstowych: zawsze, gdy użytkownik kliknie pole tekstowe, jego nazwa zostanie zapisane ukryte pole formularza:</span><span class="sxs-lookup"><span data-stu-id="21248-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="21248-119">W kodzie po stronie serwera można odczytać wartości pola ukrytego.</span><span class="sxs-lookup"><span data-stu-id="21248-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="21248-120">Ponieważ ukryte pola są proste do manipulowania, lista dozwolonych sposobem sprawdzenia ukryte wartości jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="21248-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="21248-121">Po zidentyfikowaniu pole tekstowe poprawną datę z kalendarza są zapisywane do niego.</span><span class="sxs-lookup"><span data-stu-id="21248-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


<span data-ttu-id="21248-122">[![Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="21248-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="21248-123">Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="21248-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="21248-124">[![Klikając pozycję na wartość typu date umieszcza go w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="21248-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="21248-125">Klikając pozycję na wartość typu date umieszcza go w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="21248-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="21248-126">[Poprzednie](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [dalej](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="21248-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
