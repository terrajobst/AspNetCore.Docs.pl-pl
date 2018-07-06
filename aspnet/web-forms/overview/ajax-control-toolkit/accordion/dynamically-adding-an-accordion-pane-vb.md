---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamiczne dodawanie okienka kontrolki Accordion (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są deklarowane w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 1fdb95ee1ee93bc011a257e4e21c876dbbc7d2a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820029"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="6b9b9-104">Dynamiczne dodawanie okienka kontrolki Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="6b9b9-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="6b9b9-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6b9b9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6b9b9-106">[Pobierz program Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6b9b9-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="6b9b9-107">Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="6b9b9-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="6b9b9-108">Panele zwykle są zadeklarowane w obrębie sama strona, ale kod po stronie serwera może służyć do ten sam efekt.</span><span class="sxs-lookup"><span data-stu-id="6b9b9-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="6b9b9-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6b9b9-109">Overview</span></span>

<span data-ttu-id="6b9b9-110">Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="6b9b9-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="6b9b9-111">Panele zwykle są zadeklarowane w obrębie sama strona, ale kod po stronie serwera może służyć do ten sam efekt.</span><span class="sxs-lookup"><span data-stu-id="6b9b9-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="6b9b9-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="6b9b9-112">Steps</span></span>

<span data-ttu-id="6b9b9-113">Kontrolka właściwości Accordion uwidacznia wszystkie ważne właściwości do kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="6b9b9-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="6b9b9-114">Między innymi `Panes` właściwość udziela dostępu do kolekcja okienek, które tworzą właściwości Accordion.</span><span class="sxs-lookup"><span data-stu-id="6b9b9-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="6b9b9-115">Okienka, co jest typu `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="6b9b9-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="6b9b9-116">W związku z tym jest prosta, aby utworzyć takie okienka:</span><span class="sxs-lookup"><span data-stu-id="6b9b9-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="6b9b9-117">`HeaderContainer` Właściwość `AccordionPane` zapewnia dostęp do kontrolek ASP.NET, w ramach sekcji nagłówka okienka; `ContentContainer` właściwość `AccordionPane` działa tak samo dla zawartości sekcji okienka.</span><span class="sxs-lookup"><span data-stu-id="6b9b9-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="6b9b9-118">Dzięki temu kod platformy ASP.NET do dodawania zawartości do okienka:</span><span class="sxs-lookup"><span data-stu-id="6b9b9-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="6b9b9-119">Na koniec pane(s) muszą zostać dodane do `Panes` zbiór właściwości Accordion:</span><span class="sxs-lookup"><span data-stu-id="6b9b9-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="6b9b9-120">Oto kompletny kod po stronie serwera, który dodaje dwa okienka do kontrolki właściwości Accordion:</span><span class="sxs-lookup"><span data-stu-id="6b9b9-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="6b9b9-121">Jedynym elementem brak jest właściwości Accordion, która zależy od obecności ASP.NET `ScriptManager` sterowania:</span><span class="sxs-lookup"><span data-stu-id="6b9b9-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="6b9b9-122">Aby zakończyć w przykładzie, dwóch klas CSS, do którego odwołuje się kontroli właściwości Accordion zawierają informacje dotyczące stylu przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="6b9b9-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="6b9b9-123">[![Dane w właściwości accordion dynamicznie został dodany przez kod po stronie serwera](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6b9b9-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="6b9b9-124">Dane w właściwości accordion dynamicznie został dodany przez kod po stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6b9b9-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6b9b9-125">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="6b9b9-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
