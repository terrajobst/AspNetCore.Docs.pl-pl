---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: "Tworzenie wykluczają się wzajemnie pola wyboru (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Jeżeli można wybrać tylko jeden zestaw opcji, przycisków radiowych zwykle są używane. Brak zwrotnych, choć: po wybraniu jednego przycisku radiowego w grupie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: e165c3784b246effcaeafc0ad4274bc0ca81a99c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="a1586-104">Tworzenie wykluczają się wzajemnie pola wyboru (C#)</span><span class="sxs-lookup"><span data-stu-id="a1586-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="a1586-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a1586-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a1586-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a1586-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="a1586-107">Jeżeli można wybrać tylko jeden zestaw opcji, przycisków radiowych zwykle są używane.</span><span class="sxs-lookup"><span data-stu-id="a1586-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="a1586-108">Brak zwrotnych, choć: po wybraniu jednego przycisku radiowego w grupie nie jest możliwe usunąć zaznaczenie wszystkich przycisków radiowych.</span><span class="sxs-lookup"><span data-stu-id="a1586-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="a1586-109">Pola wyboru można usunąć zaznaczenia w dowolnym momencie, ale nie wykluczają się wzajemnie.</span><span class="sxs-lookup"><span data-stu-id="a1586-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="a1586-110">Ten samouczek zawiera najlepsze cechy obu podejść: pola wyboru, które wzajemnie się wykluczają.</span><span class="sxs-lookup"><span data-stu-id="a1586-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="a1586-111">Omówienie</span><span class="sxs-lookup"><span data-stu-id="a1586-111">Overview</span></span>

<span data-ttu-id="a1586-112">Jeżeli można wybrać tylko jeden zestaw opcji, przycisków radiowych zwykle są używane.</span><span class="sxs-lookup"><span data-stu-id="a1586-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="a1586-113">Brak zwrotnych, choć: po wybraniu jednego przycisku radiowego w grupie nie jest możliwe usunąć zaznaczenie wszystkich przycisków radiowych.</span><span class="sxs-lookup"><span data-stu-id="a1586-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="a1586-114">Pola wyboru można usunąć zaznaczenia w dowolnym momencie, ale nie wykluczają się wzajemnie.</span><span class="sxs-lookup"><span data-stu-id="a1586-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="a1586-115">Ten samouczek zawiera najlepsze cechy obu podejść: pola wyboru, które wzajemnie się wykluczają.</span><span class="sxs-lookup"><span data-stu-id="a1586-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="a1586-116">Kroki</span><span class="sxs-lookup"><span data-stu-id="a1586-116">Steps</span></span>

<span data-ttu-id="a1586-117">Zestawie narzędzi programu ASP.NET AJAX formant zawiera MutuallyExclusiveCheckBox rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="a1586-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="a1586-118">Dzięki temu programiści można przypisać wszystkie pola wyboru o nazwie grupy (`Key` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="a1586-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="a1586-119">Z wszystkich pól wyboru w ramach tej samej grupy w tym samym czasie można wybrać tylko jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="a1586-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="a1586-120">Zacznijmy od umieszczania dwa pola wyboru na nowej strony ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a1586-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="a1586-121">Może istnieć więcej, ale dwa z nich wystarczające, aby zademonstrować zasady:</span><span class="sxs-lookup"><span data-stu-id="a1586-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="a1586-122">Dla oba pola wyboru formantu MutuallyExclusiveCheckBoxExtender musi znajdować się na stronie.</span><span class="sxs-lookup"><span data-stu-id="a1586-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="a1586-123">Oba atrybuty klucza muszą mieć taką samą wartość, podobnie jak wartość atrybutów elementów HTML przycisku radiowego musi być taka sama jak określenia grupy, do których należą.</span><span class="sxs-lookup"><span data-stu-id="a1586-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="a1586-124">Właściwość targetcontrolid równa punkty rozszerzeń dla identyfikatora pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="a1586-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="a1586-125">Ponadto obejmować ASP.NET AJAX `ScriptManager` wymaganych przez wszystkie elementy zestawu ASP.NET AJAX kontroli narzędzi:</span><span class="sxs-lookup"><span data-stu-id="a1586-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="a1586-126">Zapisz i uruchom strony: można sprawdzić i usuń zaznaczenie pola wyboru, ale nie oba pola wyboru można sprawdzić.</span><span class="sxs-lookup"><span data-stu-id="a1586-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="a1586-127">[![Jednocześnie można sprawdzić tylko jedno pole wyboru](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a1586-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="a1586-128">Jednocześnie można sprawdzić tylko jedno pole wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a1586-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="a1586-129">Dalej</span><span class="sxs-lookup"><span data-stu-id="a1586-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
