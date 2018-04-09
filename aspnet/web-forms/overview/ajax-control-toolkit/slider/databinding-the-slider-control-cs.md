---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Wiązania danych formantu suwaka (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość powiązania bieżące położenie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7644c991cd88868235511ba372be1f5b47c68fea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="2b26e-104">Wiązania danych formantu suwaka (C#)</span><span class="sxs-lookup"><span data-stu-id="2b26e-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="2b26e-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2b26e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2b26e-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2b26e-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="2b26e-107">Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="2b26e-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="2b26e-108">Istnieje możliwość powiązania bieżącej pozycji suwaka inny formant ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2b26e-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="2b26e-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2b26e-109">Overview</span></span>

<span data-ttu-id="2b26e-110">Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="2b26e-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="2b26e-111">Istnieje możliwość powiązania bieżącej pozycji suwaka inny formant ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2b26e-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="2b26e-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="2b26e-112">Steps</span></span>

<span data-ttu-id="2b26e-113">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="2b26e-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="2b26e-114">Następnie dodaj dwie `TextBox` formantów strony.</span><span class="sxs-lookup"><span data-stu-id="2b26e-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="2b26e-115">Jeden zostanie on przekształcony w graficznym suwaka, a jeden z nich będą przechowywane pozycji suwaka.</span><span class="sxs-lookup"><span data-stu-id="2b26e-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="2b26e-116">Następnym krokiem jest już ostatnim kroku.</span><span class="sxs-lookup"><span data-stu-id="2b26e-116">The next step is already the final step.</span></span> <span data-ttu-id="2b26e-117">`SliderExtender` Formantu z zestawu ASP.NET AJAX kontroli narzędzi powoduje, że suwak poza pierwsze pole tekstowe i automatycznie aktualizuje drugiego pola tekstowego podczas zmiany pozycji suwaka.</span><span class="sxs-lookup"><span data-stu-id="2b26e-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="2b26e-118">Do pracy, aby `SliderExtender`w `TargetControlID` atrybut musi być ustawiony na identyfikator pierwszego pola tekstowego; `BoundControlID` atrybut musi być ustawiony na identyfikator drugiego pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="2b26e-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="2b26e-119">Jak widać w przeglądarce, powiązania danych działa w obu kierunkach: wprowadzanie nową wartość w polu tekstowym aktualizuje pozycja suwaka.</span><span class="sxs-lookup"><span data-stu-id="2b26e-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="2b26e-120">Jeśli wprowadzisz drugie pole tekstowe tylko do odczytu, można dodać ochrony słabe do pola tekstowego tak, aby przeszkodę dla użytkownika ręcznie zaktualizować wartości w nim.</span><span class="sxs-lookup"><span data-stu-id="2b26e-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="2b26e-121">[![Suwak i pole tekstowe są zsynchronizowane](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2b26e-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="2b26e-122">Suwak i pole tekstowe są zsynchronizowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2b26e-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2b26e-123">[Poprzednie](using-the-slider-control-with-auto-postback-cs.md)
> [dalej](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2b26e-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
