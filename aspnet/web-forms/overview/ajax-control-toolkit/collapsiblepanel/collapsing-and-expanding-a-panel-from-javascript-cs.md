---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Rozwijanie i zwijanie panelu z poziomu języka JavaScript (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formant CollapsiblePanel w zestawie narzędzi programu ASP.NET AJAX kontroli rozszerza panelu i zapewnia możliwość zwijanie zawartością i rozwiń go...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 7baa3be7144946bde7d11afd9b1cb5f14ad9dede
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="056ab-103">Rozwijanie i zwijanie panelu z poziomu języka JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="056ab-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>
====================
<span data-ttu-id="056ab-104">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="056ab-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="056ab-105">[Pobierz kod](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="056ab-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="056ab-106">Formant CollapsiblePanel w zestawie narzędzi programu ASP.NET AJAX kontroli rozszerza panelu i zapewnia możliwość zwijanie zawartością i rozwiń go ponownie.</span><span class="sxs-lookup"><span data-stu-id="056ab-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="056ab-107">Te dwie akcje można również uruchomić z niestandardowego kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="056ab-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="056ab-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="056ab-108">Overview</span></span>

<span data-ttu-id="056ab-109">Formant CollapsiblePanel w zestawie narzędzi programu ASP.NET AJAX kontroli rozszerza panelu i zapewnia możliwość zwijanie zawartością i rozwiń go ponownie.</span><span class="sxs-lookup"><span data-stu-id="056ab-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="056ab-110">Te dwie akcje można również uruchomić z niestandardowego kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="056ab-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="056ab-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="056ab-111">Steps</span></span>

<span data-ttu-id="056ab-112">Po pierwsze, Utwórz nową stronę ASP.NET i obejmują `ScriptManager` w ramach jednej `<form>` elementu.</span><span class="sxs-lookup"><span data-stu-id="056ab-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="056ab-113">Spowoduje to załadowanie biblioteki ASP.NET AJAX, co jest wymagane przez zestaw narzędzi do sterowania:</span><span class="sxs-lookup"><span data-stu-id="056ab-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="056ab-114">Następnie należy utworzyć panelu z tekstem tak, aby były widoczne efekt Rozwiń/Zwiń:</span><span class="sxs-lookup"><span data-stu-id="056ab-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="056ab-115">Jak widać, panelu odwołuje się do klasy CSS, która jest wyświetlana w tym miejscu (i zasadniczo definiuje kolor tła i szerokość panelu):</span><span class="sxs-lookup"><span data-stu-id="056ab-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="056ab-116">`CollapsiblePanelExtender` Formant wymaga `TargetControlID` atrybutu, aby zestaw narzędzi znała panelu, które można zwijać i rozwijać na żądanie:</span><span class="sxs-lookup"><span data-stu-id="056ab-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="056ab-117">Niestety extender aktualnie nie uwidacznia interfejsu API dla zwijanie lub rozwijanie panelu, ale niektóre metody nieudokumentowanej będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="056ab-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="056ab-118">Po pierwsze Dodaj trzy przyciski HTML do strony, które następnie wyzwoli JavaScript po stronie klienta, które można zwijać i rozwijać zawartość panelu:</span><span class="sxs-lookup"><span data-stu-id="056ab-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="056ab-119">W kodzie JavaScript po stronie klienta (wprowadzenie do `<script type="text/javascript">`), `$find()` metoda musi zostać użyte do dostępu `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="056ab-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="056ab-120">`$find("cpe")` Zwraca odwołanie do niej.</span><span class="sxs-lookup"><span data-stu-id="056ab-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="056ab-121">Stamtąd na konkretnych metod rozwiąże wykonywanego zadania.</span><span class="sxs-lookup"><span data-stu-id="056ab-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="056ab-122">Metoda otwierania (rozszerzenie) jest nazywany panelu `_doOpen()`; poniższy kod implementuje `doOpen()` funkcja wywoływana po kliknięciu przycisku pierwszej:</span><span class="sxs-lookup"><span data-stu-id="056ab-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="056ab-123">Zamykanie lub zwijanie panelu `_doClose()` metody ma zostać wykonana.</span><span class="sxs-lookup"><span data-stu-id="056ab-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="056ab-124">Tak, gdy użytkownik kliknie przycisk drugiego, następujący kod JavaScript jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="056ab-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="056ab-125">Trzeci przycisk Przełącza stan panelu: z zwinięte do rozwinięta i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="056ab-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="056ab-126">`CollapsiblePanelExtender` Przedstawia `toggle()` metodę, która jest dokładnie który: odwraca stan panelu.</span><span class="sxs-lookup"><span data-stu-id="056ab-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="056ab-127">Jednak istnieje również inna metoda (wewnętrznie używany przez `toggle()` metody): `get_Collapsed()` metody `CollapsiblePanelExtender()` informuje NAS, czy panel jest zwinięty.</span><span class="sxs-lookup"><span data-stu-id="056ab-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="056ab-128">W zależności od zwracanej wartości tej funkcji, panelu jest następnie albo rozwinięty (`_doOpen()` metody) lub zwinięte (`_doClose()`) metody:</span><span class="sxs-lookup"><span data-stu-id="056ab-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


<span data-ttu-id="056ab-129">[![Trzeci przycisk zmieni stan panelu: z zwinięte rozwinięte i zapasowego](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="056ab-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="056ab-130">Trzeci przycisk zmieni stan panelu: z zwinięte rozwinięte i zapasowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="056ab-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="056ab-131">Next</span><span class="sxs-lookup"><span data-stu-id="056ab-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)
