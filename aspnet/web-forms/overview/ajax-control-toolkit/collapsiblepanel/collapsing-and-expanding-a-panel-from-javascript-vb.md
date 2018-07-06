---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Rozwijanie i zwijanie panelu z poziomu języka JavaScript (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Rozszerzenie panelu i zapewnia możliwość Zwiń jego zawartość i rozwiń go kontrolki CollapsiblePanel w ASP.NET AJAX Control Toolkit...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: da1bc6958dba99ffc5ef54fbfbc003bb26050495
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812508"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a><span data-ttu-id="5493b-103">Rozwijanie i zwijanie panelu z poziomu języka JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="5493b-103">Collapsing and Expanding a Panel from JavaScript (VB)</span></span>
====================
<span data-ttu-id="5493b-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5493b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5493b-105">[Pobierz program Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5493b-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span></span>

> <span data-ttu-id="5493b-106">Kontrolka CollapsiblePanel w ASP.NET AJAX Control Toolkit rozszerza panelu i zapewnia możliwość Zwiń jego zawartość i rozwiń go ponownie.</span><span class="sxs-lookup"><span data-stu-id="5493b-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="5493b-107">Te dwie akcje mogą być też wywoływane z niestandardowego kodu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5493b-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="5493b-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5493b-108">Overview</span></span>

<span data-ttu-id="5493b-109">Kontrolka CollapsiblePanel w ASP.NET AJAX Control Toolkit rozszerza panelu i zapewnia możliwość Zwiń jego zawartość i rozwiń go ponownie.</span><span class="sxs-lookup"><span data-stu-id="5493b-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="5493b-110">Te dwie akcje mogą być też wywoływane z niestandardowego kodu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5493b-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="5493b-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="5493b-111">Steps</span></span>

<span data-ttu-id="5493b-112">Po pierwsze, tworzenie nowej strony programu ASP.NET i zawierają `ScriptManager` w ramach jednego `<form>` elementu.</span><span class="sxs-lookup"><span data-stu-id="5493b-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="5493b-113">Spowoduje to załadowanie biblioteki ASP.NET AJAX, która jest wymagana przez zestaw narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="5493b-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="5493b-114">Następnie należy utworzyć panel zawierający tekst tak, aby były widoczne Zwiń/Rozwiń wpływu:</span><span class="sxs-lookup"><span data-stu-id="5493b-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="5493b-115">Jak widać, w panelu odwołuje się do klasy CSS, która jest wyświetlana w tym miejscu (zasadniczo Określa kolor tła i szerokość panelu):</span><span class="sxs-lookup"><span data-stu-id="5493b-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

<span data-ttu-id="5493b-116">`CollapsiblePanelExtender` Formant wymaga `TargetControlID` atrybutu, tak aby wie zestawu narzędzi, które panelu, aby zwinąć lub rozwinąć na żądanie:</span><span class="sxs-lookup"><span data-stu-id="5493b-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

<span data-ttu-id="5493b-117">Niestety urządzenia extender aktualnie nie ujawnia konkretnego interfejsu API dla zwijanie i rozwijanie panelu, ale niektóre metody nieudokumentowane będzie wykonywać.</span><span class="sxs-lookup"><span data-stu-id="5493b-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="5493b-118">Po pierwsze Dodaj trzy przyciski HTML do strony, który następnie wyzwoli JavaScript po stronie klienta, aby zwinąć lub rozwinąć panelu zawartości:</span><span class="sxs-lookup"><span data-stu-id="5493b-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="5493b-119">W przypadku kodu JavaScript po stronie klienta (wprowadzenie `<script type="text/javascript">`), `$find()` metoda musi zostać użyte do dostępu `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="5493b-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="5493b-120">`$find("cpe")` Zwraca odwołanie do niej.</span><span class="sxs-lookup"><span data-stu-id="5493b-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="5493b-121">Z tego miejsca na określonych metod rozwiąże wykonywanego zadania.</span><span class="sxs-lookup"><span data-stu-id="5493b-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="5493b-122">Metoda otwierania (Rozszerzanie) nosi nazwę panelu `_doOpen()`; poniższy kod implementuje `doOpen()` funkcja wywoływana, gdy kliknięto przycisk pierwszy:</span><span class="sxs-lookup"><span data-stu-id="5493b-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

<span data-ttu-id="5493b-123">Zamykanie lub zwijanie panelu `_doClose()` metody musi być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="5493b-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="5493b-124">Zatem gdy użytkownik kliknie drugi przycisk, następujący kod JavaScript jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="5493b-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

<span data-ttu-id="5493b-125">Trzeci przycisk Włącza/wyłącza stan panelu: od zwinięte do rozwinięta i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="5493b-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="5493b-126">`CollapsiblePanelExtender` Udostępnia `toggle()` metodę, która dokładnie tak: odwraca stan panelu.</span><span class="sxs-lookup"><span data-stu-id="5493b-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="5493b-127">Jednak istnieje również inna metoda (który wewnętrznie jest używany przez `toggle()` metoda): `get_Collapsed()` metody `CollapsiblePanelExtender()` informuje NAS, czy panel jest zwinięty.</span><span class="sxs-lookup"><span data-stu-id="5493b-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="5493b-128">W zależności od wartość zwracaną z tej funkcji jest albo rozwinięte panelu (`_doOpen()` metoda) lub zwinięte (`_doClose()`) metody:</span><span class="sxs-lookup"><span data-stu-id="5493b-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


<span data-ttu-id="5493b-129">[![Trzeci przycisk zmienia stan panelu: od zwinięte rozwinięte i Wstecz](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5493b-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="5493b-130">Trzeci przycisk zmienia stan panelu: od zwinięte rozwinięte i Wstecz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5493b-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5493b-131">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="5493b-131">Previous</span></span>](collapsing-and-expanding-a-panel-from-javascript-cs.md)
