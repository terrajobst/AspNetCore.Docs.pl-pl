---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Powiązanie danych kontrolki Slider (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy. Istnieje możliwość powiązania bieżące położenie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: aeaca2ebf61f49a5c081a3a1df188aa1541192d9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376310"
---
<a name="databinding-the-slider-control-vb"></a><span data-ttu-id="4f15d-104">Powiązanie danych kontrolki Slider (VB)</span><span class="sxs-lookup"><span data-stu-id="4f15d-104">Databinding the Slider Control (VB)</span></span>
====================
<span data-ttu-id="4f15d-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4f15d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4f15d-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4f15d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="4f15d-107">Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="4f15d-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="4f15d-108">Istnieje możliwość powiązać bieżące położenie suwaka innej kontrolki ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f15d-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="4f15d-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="4f15d-109">Overview</span></span>

<span data-ttu-id="4f15d-110">Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy.</span><span class="sxs-lookup"><span data-stu-id="4f15d-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="4f15d-111">Istnieje możliwość powiązać bieżące położenie suwaka innej kontrolki ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f15d-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="4f15d-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="4f15d-112">Steps</span></span>

<span data-ttu-id="4f15d-113">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):</span><span class="sxs-lookup"><span data-stu-id="4f15d-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="4f15d-114">Następnie dodaj dwa `TextBox` kontrolek do strony.</span><span class="sxs-lookup"><span data-stu-id="4f15d-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="4f15d-115">Jeden zostanie przekształcony w graficznym suwaka, a drugi będzie przechowywać położenie suwaka.</span><span class="sxs-lookup"><span data-stu-id="4f15d-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="4f15d-116">Następnym krokiem jest już ostatni krok.</span><span class="sxs-lookup"><span data-stu-id="4f15d-116">The next step is already the final step.</span></span> <span data-ttu-id="4f15d-117">`SliderExtender` Formantu z ASP.NET AJAX Control Toolkit sprawia, że suwak poza pierwszym polu tekstowym i automatycznie aktualizuje drugie pole tekstowe, gdy suwak pozycji zmiany.</span><span class="sxs-lookup"><span data-stu-id="4f15d-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="4f15d-118">W kolejności, w tym do pracy `SliderExtender`firmy `TargetControlID` atrybutu musi być ustawione na identyfikator pierwszego pola tekstowego; `BoundControlID` atrybutu musi być ustawione na identyfikator drugiego pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="4f15d-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="4f15d-119">Jak widać w przeglądarce, powiązanie danych działa w obu kierunkach: wprowadzając nową wartość w polu tekstowym aktualizuje położenie suwaka.</span><span class="sxs-lookup"><span data-stu-id="4f15d-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="4f15d-120">Jeśli zostało zgłoszone drugie pole tekstowe tylko do odczytu, można dodać słabe ochrony do pola tekstowego, aby jest trudniejsze dla użytkownika ręcznie zaktualizować wartość w miejscu.</span><span class="sxs-lookup"><span data-stu-id="4f15d-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="4f15d-121">[![Suwaka i pole tekstowe są zsynchronizowane](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4f15d-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="4f15d-122">Suwaka i pole tekstowe są zsynchronizowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4f15d-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4f15d-123">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="4f15d-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)
