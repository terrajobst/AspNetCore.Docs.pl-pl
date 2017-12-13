---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: "Obsługa ogłaszania zwrotnego przez formant okna podręcznego bez elementu UpdatePanel (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Podczas odświeżania strony występuje w su..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="b1bc9-104">Obsługa ogłaszania zwrotnego przez formant okna podręcznego bez elementu UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="b1bc9-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="b1bc9-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b1bc9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b1bc9-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b1bc9-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="b1bc9-107">Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="b1bc9-108">Podczas odświeżania strony odbywa się w takich panelu i na stronie istnieje kilka paneli trudno jest określić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="b1bc9-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b1bc9-109">Overview</span></span>

<span data-ttu-id="b1bc9-110">Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="b1bc9-111">Podczas odświeżania strony odbywa się w takich panelu i na stronie istnieje kilka paneli trudno jest określić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="b1bc9-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="b1bc9-112">Steps</span></span>

<span data-ttu-id="b1bc9-113">Korzystając z `PopupControl` odświeżenie strony, ale bez konieczności `UpdatePanel` na stronie Toolkit formantu nie oferuje możliwość określenia, który element klienta wyzwolił menu podręczne, które z kolei spowodowało ogłaszania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="b1bc9-114">Jednak małych lewy dostarcza obejście dla tego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="b1bc9-115">Po pierwsze, w tym miejscu jest podstawowa konfiguracja: dwa pola tekstowe podlegających zarówno samego menu podręczne kalendarza.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="b1bc9-116">Dwa `PopupControlExtenders` zebranie okna podręczne oraz pól tekstowych.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="b1bc9-117">Podstawowe rozwiązaniem jest dodanie ukryte pole formularza w &lt; `form` &gt; element, który zawiera pole tekstowe, która została uruchomiona menu podręcznego:</span><span class="sxs-lookup"><span data-stu-id="b1bc9-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="b1bc9-118">Podczas ładowania strony kodu JavaScript dodaje program obsługi zdarzeń do obu polach tekstowych: gdy użytkownik kliknie w polu tekstowym, jego nazwa jest zapisywany w ukryte pole formularza:</span><span class="sxs-lookup"><span data-stu-id="b1bc9-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="b1bc9-119">W kodzie po stronie serwera można odczytać wartości pola ukrytego.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="b1bc9-120">Ponieważ ukryte pola są prosta do manipulowania, podejście listy dozwolonych adresów IP można sprawdzić poprawności wartości hidden jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="b1bc9-121">Po zidentyfikowaniu pola tekstowego poprawną datę z kalendarza są zapisywane w nim.</span><span class="sxs-lookup"><span data-stu-id="b1bc9-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


<span data-ttu-id="b1bc9-122">[![Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b1bc9-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="b1bc9-123">Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b1bc9-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="b1bc9-124">[![Kliknij datę umieszcza je w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="b1bc9-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="b1bc9-125">Kliknij datę umieszcza je w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b1bc9-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b1bc9-126">[Poprzednie](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[dalej](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b1bc9-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
