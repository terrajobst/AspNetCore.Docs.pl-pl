---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Tworzenie kontrolki Rating (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności, oferują użytkownikom na szybkość artykuły lub elementów. Zwykle wymaga to pewnych wysiłków związanych z kodowaniem, ale mamy...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: cb8bd8ab400bf5633b8218e724ad86cf7271699d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811976"
---
<a name="creating-a-rating-control-vb"></a><span data-ttu-id="246ca-104">Tworzenie kontrolki Rating (VB)</span><span class="sxs-lookup"><span data-stu-id="246ca-104">Creating a Rating Control (VB)</span></span>
====================
<span data-ttu-id="246ca-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="246ca-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="246ca-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="246ca-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span></span>

> <span data-ttu-id="246ca-107">Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności, oferują użytkownikom na szybkość artykuły lub elementów.</span><span class="sxs-lookup"><span data-stu-id="246ca-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="246ca-108">Zwykle wymaga to pewnych wysiłków związanych z kodowaniem, ale musimy Toolkit kontroli naszych usuwania.</span><span class="sxs-lookup"><span data-stu-id="246ca-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="246ca-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="246ca-109">Overview</span></span>

<span data-ttu-id="246ca-110">Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności, oferują użytkownikom na szybkość artykuły lub elementów.</span><span class="sxs-lookup"><span data-stu-id="246ca-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="246ca-111">Zwykle wymaga to pewnych wysiłków związanych z kodowaniem, ale musimy Toolkit kontroli naszych usuwania.</span><span class="sxs-lookup"><span data-stu-id="246ca-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="246ca-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="246ca-112">Steps</span></span>

<span data-ttu-id="246ca-113">Przede wszystkim należy (co najmniej) dwa rodzaje obrazy: jeden dla wypełnienia elementu ocenę i jednego elementu pustego klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="246ca-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="246ca-114">Element Klasyfikacja jest zazwyczaj gwiazdka lub uśmiechniętą.</span><span class="sxs-lookup"><span data-stu-id="246ca-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="246ca-115">W tym scenariuszu możesz znaleźć trzy pliki, smiley.png i empty.png i done.png uśmiech jako część pobierania kodu źródłowego w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="246ca-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="246ca-116">Następnie utwórz nowy plik platformy ASP.NET i Rozpocznij od dodania `ScriptManager` do niej kontrolkę:</span><span class="sxs-lookup"><span data-stu-id="246ca-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

<span data-ttu-id="246ca-117">Następnie należy dodać `Rating` formantu z ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="246ca-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="246ca-118">Następujące atrybuty muszą zostać ustawione dla tego przykładu:</span><span class="sxs-lookup"><span data-stu-id="246ca-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="246ca-119">`CurrentRating` początkowa klasyfikacja ma być używany</span><span class="sxs-lookup"><span data-stu-id="246ca-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="246ca-120">`MaxRating` Maksymalny poziom klasyfikacji</span><span class="sxs-lookup"><span data-stu-id="246ca-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="246ca-121">`EmptyStarCssClass` Klasa CSS do użycia podczas element ranking (Star czyli rejestr) jest pusty</span><span class="sxs-lookup"><span data-stu-id="246ca-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="246ca-122">`FilledStarCssClass` Klasa CSS do użycia podczas wypełniania elementu klasyfikacji (star)</span><span class="sxs-lookup"><span data-stu-id="246ca-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="246ca-123">`StarCssClass` Klasa CSS na potrzeby stat widoczne</span><span class="sxs-lookup"><span data-stu-id="246ca-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="246ca-124">`WaitingStarCssClass` Klasa CSS do użycia podczas oceny w formie gwiazdek jest wysyłane z powrotem do serwera</span><span class="sxs-lookup"><span data-stu-id="246ca-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="246ca-125">A Oto znaczników, który tworzy kontrolki rating z pięciu elementów (smileys), których brak jest wypełniane początkowo:</span><span class="sxs-lookup"><span data-stu-id="246ca-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

<span data-ttu-id="246ca-126">Trzy klasy CSS do którego istnieje odwołanie teraz chcesz pokazać pliki odpowiedni obraz, który jest łatwo zrobić przy użyciu CSS:</span><span class="sxs-lookup"><span data-stu-id="246ca-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

<span data-ttu-id="246ca-127">Upewnij się, że podajesz, szerokość i wysokość trzy obrazy, w przeciwnym razie wyświetlanie mogą wyglądać nieco messed w górę.</span><span class="sxs-lookup"><span data-stu-id="246ca-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="246ca-128">Na koniec wynik klasyfikacji powinny być wyświetlane użytkownikowi (lub co najmniej zapisane w bazie danych).</span><span class="sxs-lookup"><span data-stu-id="246ca-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="246ca-129">Dlatego należy dodać etykietę dla danych wyjściowych, wiadomości SMS i przycisk Prześlij, aby ponownie post formularza klasyfikacji do serwera:</span><span class="sxs-lookup"><span data-stu-id="246ca-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

<span data-ttu-id="246ca-130">W kodzie po stronie serwera, dostępu do kontrolki oceny za pośrednictwem jego `ID` i uzyskuje dostęp do jego `CurrentRating` właściwość, która jest liczbą elementów wybranych klasyfikacji, w tym przykładzie wartość z zakresu od 0 do 5.</span><span class="sxs-lookup"><span data-stu-id="246ca-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

<span data-ttu-id="246ca-131">Zapisz stronę i załadować je bezpośrednio w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="246ca-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="246ca-132">Po najechaniu kursorem na elementy klasyfikacji (początkowo pusta), efekt JavaScript pojawia się: zmiany klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="246ca-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="246ca-133">Po kliknięciu zestaw gwiazdek bieżąca ocena są zachowywane.</span><span class="sxs-lookup"><span data-stu-id="246ca-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="246ca-134">Na koniec po przesłaniu formularza danych wyjściowych wybranej klasyfikacji kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="246ca-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="246ca-135">[![Tworzenie systemu klasyfikacji za pomocą minimalnej ilości kodu](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="246ca-135">[![Creating a rating system with minimal code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="246ca-136">Tworzenie systemu klasyfikacji za pomocą minimalnej ilości kodu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-rating-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="246ca-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="246ca-137">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="246ca-137">Previous</span></span>](creating-a-rating-control-cs.md)
