---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Używanie kontrolki Slider z automatycznym ogłaszaniem zwrotnym (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy. Jest możliwe automatycznie Księguj suwaka...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ddc5b119a7f58b4d289f11e1789cf193870ae4e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374042"
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="8fdb8-104">Używanie kontrolki Slider z automatycznym ogłaszaniem zwrotnym (VB)</span><span class="sxs-lookup"><span data-stu-id="8fdb8-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="8fdb8-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8fdb8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8fdb8-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8fdb8-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="8fdb8-107">Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="8fdb8-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="8fdb8-108">Istnieje możliwość zmiany autopostback suwaka po jego wartość.</span><span class="sxs-lookup"><span data-stu-id="8fdb8-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="8fdb8-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="8fdb8-109">Overview</span></span>

<span data-ttu-id="8fdb8-110">Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="8fdb8-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="8fdb8-111">Istnieje możliwość zmiany autopostback suwaka po jego wartość.</span><span class="sxs-lookup"><span data-stu-id="8fdb8-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="8fdb8-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="8fdb8-112">Steps</span></span>

<span data-ttu-id="8fdb8-113">Aby wprowadzić suwak automatycznie zwrotu na zmianę, obu polach tekstowych muszą atrybut `AutoPostBack="true"`: pole tekstowe, które staną się suwak i pola tekstowego, który przechowuje położenie suwaka.</span><span class="sxs-lookup"><span data-stu-id="8fdb8-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="8fdb8-114">W tym miejscu jest wymagane w tym:</span><span class="sxs-lookup"><span data-stu-id="8fdb8-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="8fdb8-115">`SliderExtender` Formantu z ASP.NET AJAX Control Toolkit przypisuje funkcji suwaka do dwóch pól tekstowych:</span><span class="sxs-lookup"><span data-stu-id="8fdb8-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="8fdb8-116">Element label dodatkowe posłuży później poinformowania użytkownika o odświeżenie strony:</span><span class="sxs-lookup"><span data-stu-id="8fdb8-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="8fdb8-117">Na koniec `ScriptManager` kontroli ASP.NET AJAX ładuje wymagany język JavaScript dla kontrolki zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="8fdb8-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="8fdb8-118">Teraz wrócili, publikuje suwaka po stronie serwera to zdarzenie może być przechwycony i podjęto jakieś działanie:</span><span class="sxs-lookup"><span data-stu-id="8fdb8-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="8fdb8-119">[![Przesunięcie suwaka wyzwala ogłaszania zwrotnego](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8fdb8-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="8fdb8-120">Przesunięcie suwaka wyzwala ogłaszania zwrotnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8fdb8-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="8fdb8-121">[![Później Data zmiany są zapisywane w etykiecie](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8fdb8-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="8fdb8-122">Później, Data zmiany są zapisywane w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8fdb8-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8fdb8-123">[Poprzednie](databinding-the-slider-control-cs.md)
> [dalej](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8fdb8-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
