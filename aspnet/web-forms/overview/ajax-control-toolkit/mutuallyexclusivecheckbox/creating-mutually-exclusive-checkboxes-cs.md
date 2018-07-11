---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Tworzenie wzajemnie wykluczających się pól wyboru (C#) | Dokumentacja firmy Microsoft
author: wenz
description: 'Jeżeli można wybrać tylko jeden zestaw opcji, przyciski radiowe zwykle są używane. Istnieje jednak wadą: po wybraniu jednego przycisku radiowego w grupie...'
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 1bce8cd94ed11551f75e48b19cd9cc50b9d023c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818070"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="dbd20-104">Tworzenie wzajemnie wykluczających się pól wyboru (C#)</span><span class="sxs-lookup"><span data-stu-id="dbd20-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="dbd20-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dbd20-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dbd20-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="dbd20-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="dbd20-107">Jeżeli można wybrać tylko jeden zestaw opcji, przyciski radiowe zwykle są używane.</span><span class="sxs-lookup"><span data-stu-id="dbd20-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="dbd20-108">Istnieje jednak wadą: po wybraniu jednego przycisku radiowego w grupie nie jest możliwe usunąć zaznaczenie wszystkich przycisków radiowych.</span><span class="sxs-lookup"><span data-stu-id="dbd20-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="dbd20-109">Pola wyboru mogą być zaznaczone w dowolnym momencie, jednak nie są wzajemnie się wykluczają.</span><span class="sxs-lookup"><span data-stu-id="dbd20-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="dbd20-110">Ten samouczek zawiera najlepsze cechy obu podejść: pola wyboru, które wzajemnie się wykluczają.</span><span class="sxs-lookup"><span data-stu-id="dbd20-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="dbd20-111">Omówienie</span><span class="sxs-lookup"><span data-stu-id="dbd20-111">Overview</span></span>

<span data-ttu-id="dbd20-112">Jeżeli można wybrać tylko jeden zestaw opcji, przyciski radiowe zwykle są używane.</span><span class="sxs-lookup"><span data-stu-id="dbd20-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="dbd20-113">Istnieje jednak wadą: po wybraniu jednego przycisku radiowego w grupie nie jest możliwe usunąć zaznaczenie wszystkich przycisków radiowych.</span><span class="sxs-lookup"><span data-stu-id="dbd20-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="dbd20-114">Pola wyboru mogą być zaznaczone w dowolnym momencie, jednak nie są wzajemnie się wykluczają.</span><span class="sxs-lookup"><span data-stu-id="dbd20-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="dbd20-115">Ten samouczek zawiera najlepsze cechy obu podejść: pola wyboru, które wzajemnie się wykluczają.</span><span class="sxs-lookup"><span data-stu-id="dbd20-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="dbd20-116">Kroki</span><span class="sxs-lookup"><span data-stu-id="dbd20-116">Steps</span></span>

<span data-ttu-id="dbd20-117">ASP.NET AJAX Control Toolkit zawiera MutuallyExclusiveCheckBox urządzenia extender.</span><span class="sxs-lookup"><span data-stu-id="dbd20-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="dbd20-118">Dzięki temu programiści przypisać wszystkie pola wyboru do nazwy grupy (`Key` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="dbd20-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="dbd20-119">Z wszystkich pól wyboru w ramach tej samej grupy w tym samym czasie można wybrać tylko jeden.</span><span class="sxs-lookup"><span data-stu-id="dbd20-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="dbd20-120">Zacznijmy od umieszczenie dwa pola wyboru na nowej stronie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="dbd20-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="dbd20-121">Może istnieć więcej, ale dwa z nich wystarczające, aby zademonstrować zasady:</span><span class="sxs-lookup"><span data-stu-id="dbd20-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="dbd20-122">Oba pola wyboru formantu MutuallyExclusiveCheckBoxExtender musi być wprowadzane na stronie.</span><span class="sxs-lookup"><span data-stu-id="dbd20-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="dbd20-123">Obu atrybutów klucza muszą mieć taką samą wartość, podobnie jak wartość atrybutów elementów HTML przycisku radiowego musi być taka sama jak oznaczenia grupy, do których należą.</span><span class="sxs-lookup"><span data-stu-id="dbd20-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="dbd20-124">Właściwość TargetControlID punktów rozszerzeń identyfikator pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="dbd20-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="dbd20-125">Na koniec obejmują ASP.NET AJAX `ScriptManager` co jest wymagane przez wszystkie elementy ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="dbd20-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="dbd20-126">Zapisz i uruchom strony: sprawdza, czy i usuń zaznaczenie pola wyboru, jednak w żadnym momencie oba pola wyboru można sprawdzić.</span><span class="sxs-lookup"><span data-stu-id="dbd20-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="dbd20-127">[![Można sprawdzić w danym momencie tylko jedno pole wyboru](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dbd20-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="dbd20-128">Można sprawdzić w danym momencie tylko jedno pole wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dbd20-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dbd20-129">Next</span><span class="sxs-lookup"><span data-stu-id="dbd20-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
