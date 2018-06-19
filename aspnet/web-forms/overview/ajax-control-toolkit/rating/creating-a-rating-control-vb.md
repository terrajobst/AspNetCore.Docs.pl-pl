---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Tworzenie formantu klasyfikacji (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności oferuje użytkownikom artykuły szybkość lub elementów. Zazwyczaj wymaga niektórych kodowania wysiłku, ale mamy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c19f36dfe1b72a3954db61ff1845e99c02e47c14
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868415"
---
<a name="creating-a-rating-control-vb"></a><span data-ttu-id="8c11f-104">Tworzenie formantu klasyfikacji (VB)</span><span class="sxs-lookup"><span data-stu-id="8c11f-104">Creating a Rating Control (VB)</span></span>
====================
<span data-ttu-id="8c11f-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8c11f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8c11f-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8c11f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span></span>

> <span data-ttu-id="8c11f-107">Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności oferuje użytkownikom artykuły szybkość lub elementów.</span><span class="sxs-lookup"><span data-stu-id="8c11f-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="8c11f-108">Zazwyczaj wymaga niektórych kodowania wysiłku, ale mamy kontroli zestawu narzędzi do naszej usuwania.</span><span class="sxs-lookup"><span data-stu-id="8c11f-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="8c11f-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="8c11f-109">Overview</span></span>

<span data-ttu-id="8c11f-110">Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności oferuje użytkownikom artykuły szybkość lub elementów.</span><span class="sxs-lookup"><span data-stu-id="8c11f-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="8c11f-111">Zazwyczaj wymaga niektórych kodowania wysiłku, ale mamy kontroli zestawu narzędzi do naszej usuwania.</span><span class="sxs-lookup"><span data-stu-id="8c11f-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="8c11f-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="8c11f-112">Steps</span></span>

<span data-ttu-id="8c11f-113">Przede wszystkim należy (co najmniej) dwa rodzaje obrazy: jeden dla wypełniony element ranking i jeden dla elementu pusta klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="8c11f-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="8c11f-114">Element ranking jest zwykle gwiazdy lub uśmiech.</span><span class="sxs-lookup"><span data-stu-id="8c11f-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="8c11f-115">W przypadku tego scenariusza można znaleźć trzy pliki, smiley.png i empty.png oraz done.png uśmiech jako część Pobieranie kodu źródłowego dla tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="8c11f-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="8c11f-116">Następnie utwórz nowy plik programu ASP.NET i Rozpocznij od dodania `ScriptManager` formantu do niej:</span><span class="sxs-lookup"><span data-stu-id="8c11f-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

<span data-ttu-id="8c11f-117">Następnie należy dodać `Rating` formantu w zestawie narzędzi programu ASP.NET AJAX formantu.</span><span class="sxs-lookup"><span data-stu-id="8c11f-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="8c11f-118">Następujące atrybuty muszą zostać ustawione dla tego przykładu:</span><span class="sxs-lookup"><span data-stu-id="8c11f-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="8c11f-119">`CurrentRating` początkowa klasyfikacji do użycia</span><span class="sxs-lookup"><span data-stu-id="8c11f-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="8c11f-120">`MaxRating` Maksymalny dozwolony poziom klasyfikacji</span><span class="sxs-lookup"><span data-stu-id="8c11f-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="8c11f-121">`EmptyStarCssClass` Klasa CSS do użycia podczas elementu klasyfikacji (gwiazdkę) jest pusty</span><span class="sxs-lookup"><span data-stu-id="8c11f-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="8c11f-122">`FilledStarCssClass` Klasa CSS do użycia podczas wypełniania elementu klasyfikacji (gwiazdkę)</span><span class="sxs-lookup"><span data-stu-id="8c11f-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="8c11f-123">`StarCssClass` Klasa CSS do użycia na potrzeby stat widoczne</span><span class="sxs-lookup"><span data-stu-id="8c11f-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="8c11f-124">`WaitingStarCssClass` Klasa CSS do użycia podczas gwiazdki są wysyłane do serwera</span><span class="sxs-lookup"><span data-stu-id="8c11f-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="8c11f-125">A Oto kod znaczników, który tworzy kontrolkę klasyfikacji z pięciu elementów (smileys), których brak wypełniania początkowo:</span><span class="sxs-lookup"><span data-stu-id="8c11f-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

<span data-ttu-id="8c11f-126">Teraz trzech klas CSS przywoływanego należy Pokaż pliki odpowiedni obraz, który jest łatwo zrobić przy użyciu CSS:</span><span class="sxs-lookup"><span data-stu-id="8c11f-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

<span data-ttu-id="8c11f-127">Upewnij się, że Podaj szerokość i wysokość trzy obrazy, w przeciwnym razie wyświetlenia może wyglądać nieco messed w górę.</span><span class="sxs-lookup"><span data-stu-id="8c11f-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="8c11f-128">Na koniec wynik Klasyfikacja powinien wyświetlane użytkownikowi (lub, co najmniej zapisane w bazie danych).</span><span class="sxs-lookup"><span data-stu-id="8c11f-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="8c11f-129">Dlatego Dodaj etykietę dla danych wyjściowych wiadomości SMS i przycisk Prześlij post-back formularza klasyfikacji na serwerze:</span><span class="sxs-lookup"><span data-stu-id="8c11f-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

<span data-ttu-id="8c11f-130">W kodzie po stronie serwera, dostępu formantu klasyfikacji za pośrednictwem jego `ID` i uzyskuje dostęp do jego `CurrentRating` właściwość, która jest liczba elementów wybranej klasyfikacji, w tym przykładzie wartość z zakresu od 0 do 5.</span><span class="sxs-lookup"><span data-stu-id="8c11f-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

<span data-ttu-id="8c11f-131">Strony i załadować go w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="8c11f-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="8c11f-132">Po umieszczeniu na elementy klasyfikacji (początkowo puste), występuje efekt JavaScript: zmiany klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="8c11f-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="8c11f-133">Po kliknięciu zestaw gwiazdek bieżąca ocena są zachowywane.</span><span class="sxs-lookup"><span data-stu-id="8c11f-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="8c11f-134">Na koniec po przesłaniu formularza kod po stronie serwera generuje wybranej klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="8c11f-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="8c11f-135">[![Tworzenie system klasyfikacji z minimalnym kodu](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8c11f-135">[![Creating a rating system with minimal code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="8c11f-136">Tworzenie system klasyfikacji z minimalnym kodu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-rating-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8c11f-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8c11f-137">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="8c11f-137">Previous</span></span>](creating-a-rating-control-cs.md)
