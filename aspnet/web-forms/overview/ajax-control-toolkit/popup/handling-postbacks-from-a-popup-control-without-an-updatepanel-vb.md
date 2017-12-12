---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: "Obsługa ogłaszania zwrotnego przez formant okna podręcznego bez elementu UpdatePanel (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Podczas odświeżania strony występuje w su..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c4afee37eab33036e5e563e78f873275951700b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="4acd4-104">Obsługa ogłaszania zwrotnego przez formant okna podręcznego bez elementu UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="4acd4-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="4acd4-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4acd4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4acd4-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4acd4-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="4acd4-107">Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="4acd4-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="4acd4-108">Podczas odświeżania strony odbywa się w takich panelu i na stronie istnieje kilka paneli trudno jest określić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="4acd4-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="4acd4-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4acd4-109">Overview</span></span>

<span data-ttu-id="4acd4-110">Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny.</span><span class="sxs-lookup"><span data-stu-id="4acd4-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="4acd4-111">Podczas odświeżania strony odbywa się w takich panelu i na stronie istnieje kilka paneli trudno jest określić, który panel został kliknięty.</span><span class="sxs-lookup"><span data-stu-id="4acd4-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="4acd4-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="4acd4-112">Steps</span></span>

<span data-ttu-id="4acd4-113">Korzystając z `PopupControl` odświeżenie strony, ale bez konieczności `UpdatePanel` na stronie Toolkit formantu nie oferuje możliwość określenia, który element klienta wyzwolił menu podręczne, które z kolei spowodowało ogłaszania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="4acd4-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="4acd4-114">Jednak małych lewy dostarcza obejście dla tego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="4acd4-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="4acd4-115">Po pierwsze, w tym miejscu jest podstawowa konfiguracja: dwa pola tekstowe podlegających zarówno samego menu podręczne kalendarza.</span><span class="sxs-lookup"><span data-stu-id="4acd4-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="4acd4-116">Dwa `PopupControlExtenders` zebranie okna podręczne oraz pól tekstowych.</span><span class="sxs-lookup"><span data-stu-id="4acd4-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="4acd4-117">Podstawowe rozwiązaniem jest dodanie ukryte pole formularza w &lt; `form` &gt; element, który zawiera pole tekstowe, która została uruchomiona menu podręcznego:</span><span class="sxs-lookup"><span data-stu-id="4acd4-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="4acd4-118">Podczas ładowania strony kodu JavaScript dodaje program obsługi zdarzeń do obu polach tekstowych: gdy użytkownik kliknie w polu tekstowym, jego nazwa jest zapisywany w ukryte pole formularza:</span><span class="sxs-lookup"><span data-stu-id="4acd4-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="4acd4-119">W kodzie po stronie serwera można odczytać wartości pola ukrytego.</span><span class="sxs-lookup"><span data-stu-id="4acd4-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="4acd4-120">Ponieważ ukryte pola są prosta do manipulowania, podejście listy dozwolonych adresów IP można sprawdzić poprawności wartości hidden jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="4acd4-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="4acd4-121">Po zidentyfikowaniu pola tekstowego poprawną datę z kalendarza są zapisywane w nim.</span><span class="sxs-lookup"><span data-stu-id="4acd4-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


<span data-ttu-id="4acd4-122">[![Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4acd4-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="4acd4-123">Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4acd4-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="4acd4-124">[![Kliknij datę umieszcza je w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4acd4-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="4acd4-125">Kliknij datę umieszcza je w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4acd4-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="4acd4-126">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="4acd4-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
