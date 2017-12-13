---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Dynamiczne dodawanie Harmonijka okienka (C#) | Dokumentacja firmy Microsoft
author: wenz
description: "Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie. Panele są zwykle deklarowanych w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c6af79186ca21082647beec500c47974a47c35a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="d5d29-104">Dynamiczne dodawanie Harmonijka okienka (C#)</span><span class="sxs-lookup"><span data-stu-id="d5d29-104">Dynamically Adding An Accordion Pane (C#)</span></span>
====================
<span data-ttu-id="d5d29-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d5d29-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d5d29-106">[Pobierz kod](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d5d29-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="d5d29-107">Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie.</span><span class="sxs-lookup"><span data-stu-id="d5d29-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="d5d29-108">Panele są zwykle deklarowanych w samej strony, ale kod po stronie serwera może być używany do osiągnąć ten sam rezultat.</span><span class="sxs-lookup"><span data-stu-id="d5d29-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="d5d29-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d5d29-109">Overview</span></span>

<span data-ttu-id="d5d29-110">Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie.</span><span class="sxs-lookup"><span data-stu-id="d5d29-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="d5d29-111">Panele są zwykle deklarowanych w samej strony, ale kod po stronie serwera może być używany do osiągnąć ten sam rezultat.</span><span class="sxs-lookup"><span data-stu-id="d5d29-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="d5d29-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="d5d29-112">Steps</span></span>

<span data-ttu-id="d5d29-113">Formant Accordion udostępnia wszystkie ważne właściwości do kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="d5d29-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="d5d29-114">Między innymi `Panes` właściwość udziela dostępu do kolekcji okienka wchodzące w skład Accordion.</span><span class="sxs-lookup"><span data-stu-id="d5d29-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="d5d29-115">Okienko, co jest typu `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="d5d29-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="d5d29-116">W związku z tym jest prosta, aby utworzyć takie okienka:</span><span class="sxs-lookup"><span data-stu-id="d5d29-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="d5d29-117">`HeaderContainer` Właściwość `AccordionPane` zapewnia dostęp do formantów ASP.NET w sekcji nagłówka okienka; `ContentContainer` właściwość `AccordionPane` jest taki sam dla zawartości sekcji okienka.</span><span class="sxs-lookup"><span data-stu-id="d5d29-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="d5d29-118">Dzięki temu kodu platformy ASP.NET można dodać zawartości do okienka:</span><span class="sxs-lookup"><span data-stu-id="d5d29-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="d5d29-119">Na koniec pane(s) musi zostać dodany do `Panes` kolekcji Accordion:</span><span class="sxs-lookup"><span data-stu-id="d5d29-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="d5d29-120">W tym miejscu jest kompletny kod po stronie serwera, który dodaje dwa okienka do kontrolki Accordion:</span><span class="sxs-lookup"><span data-stu-id="d5d29-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="d5d29-121">Accordion, która zależy od obecności ASP.NET jest jedynym elementem Brak `ScriptManager` sterowania:</span><span class="sxs-lookup"><span data-stu-id="d5d29-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="d5d29-122">Aby zakończyć w przykładzie, dwie klasy CSS, do którego odwołuje się formant Accordion dostarcza informacji o stylu przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="d5d29-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


<span data-ttu-id="d5d29-123">[![Dane w accordion dynamicznie została dodana przez kod po stronie serwera](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d5d29-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="d5d29-124">Dane w accordion dynamicznie została dodana przez kod po stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d5d29-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d5d29-125">[Poprzednie](databinding-to-an-accordion-cs.md)
[dalej](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d5d29-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
