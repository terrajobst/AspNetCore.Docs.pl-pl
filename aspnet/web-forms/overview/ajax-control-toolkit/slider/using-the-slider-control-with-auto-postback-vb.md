---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: "Za pomocą suwaka z automatycznego odświeżania strony (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość automatycznie Księguj suwaka..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: fedd3ff947c6f5d5d4d00791087e9fd825fdf9d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="8fa00-104">Za pomocą suwaka z automatycznego odświeżania strony (VB)</span><span class="sxs-lookup"><span data-stu-id="8fa00-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="8fa00-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8fa00-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8fa00-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8fa00-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="8fa00-107">Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="8fa00-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="8fa00-108">Istnieje możliwość zmiany autopostback suwaka po jego wartość.</span><span class="sxs-lookup"><span data-stu-id="8fa00-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="8fa00-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="8fa00-109">Overview</span></span>

<span data-ttu-id="8fa00-110">Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="8fa00-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="8fa00-111">Istnieje możliwość zmiany autopostback suwaka po jego wartość.</span><span class="sxs-lookup"><span data-stu-id="8fa00-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="8fa00-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="8fa00-112">Steps</span></span>

<span data-ttu-id="8fa00-113">W celu suwak automatycznego odświeżania strony na zmianę, obu polach tekstowych wymagają atrybutu `AutoPostBack="true"`: pola tekstowego, który ma zostać suwak sam i pola tekstowego, która przechowuje pozycji suwaka.</span><span class="sxs-lookup"><span data-stu-id="8fa00-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="8fa00-114">Oto wymagane znacznika, w tym:</span><span class="sxs-lookup"><span data-stu-id="8fa00-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="8fa00-115">`SliderExtender` Formantu w zestawie narzędzi programu ASP.NET AJAX kontroli przypisuje funkcji suwak w polach tekstowych:</span><span class="sxs-lookup"><span data-stu-id="8fa00-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="8fa00-116">Element label dodatkowe później będzie służyć do poinformowania użytkownika odświeżania strony:</span><span class="sxs-lookup"><span data-stu-id="8fa00-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="8fa00-117">Na koniec `ScriptManager` ładowany formant programu ASP.NET AJAX JavaScript wymagane dla kontroli zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="8fa00-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="8fa00-118">Teraz suwak jest przesyłanie ponownie; po stronie serwera to zdarzenie może być przechwycono i reagować:</span><span class="sxs-lookup"><span data-stu-id="8fa00-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="8fa00-119">[![Przesunięcie suwaka wyzwala odświeżania strony](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8fa00-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="8fa00-120">Przesunięcie suwaka wyzwala odświeżania strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8fa00-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="8fa00-121">[![Później Data zmiany są zapisywane w etykiecie](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8fa00-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="8fa00-122">Później, Data zmiany są zapisywane w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8fa00-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8fa00-123">[Poprzednie](databinding-the-slider-control-cs.md)
[dalej](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8fa00-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
