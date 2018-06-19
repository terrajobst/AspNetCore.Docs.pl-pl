---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Tworzenie formantu klasyfikacji (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności oferuje użytkownikom artykuły szybkość lub elementów. Zazwyczaj wymaga niektórych kodowania wysiłku, ale mamy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a48cf0ed9402e2875e87ba7bdb76afc5f501a670
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879598"
---
<a name="creating-a-rating-control-c"></a>Tworzenie formantu klasyfikacji (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności oferuje użytkownikom artykuły szybkość lub elementów. Zazwyczaj wymaga niektórych kodowania wysiłku, ale mamy kontroli zestawu narzędzi do naszej usuwania.


## <a name="overview"></a>Omówienie

Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności oferuje użytkownikom artykuły szybkość lub elementów. Zazwyczaj wymaga niektórych kodowania wysiłku, ale mamy kontroli zestawu narzędzi do naszej usuwania.

## <a name="steps"></a>Kroki

Przede wszystkim należy (co najmniej) dwa rodzaje obrazy: jeden dla wypełniony element ranking i jeden dla elementu pusta klasyfikacji. Element ranking jest zwykle gwiazdy lub uśmiech. W przypadku tego scenariusza można znaleźć trzy pliki, smiley.png i empty.png oraz done.png uśmiech jako część Pobieranie kodu źródłowego dla tego samouczka.

Następnie utwórz nowy plik programu ASP.NET i Rozpocznij od dodania `ScriptManager` formantu do niej:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

Następnie należy dodać `Rating` formantu w zestawie narzędzi programu ASP.NET AJAX formantu. Następujące atrybuty muszą zostać ustawione dla tego przykładu:

- `CurrentRating` początkowa klasyfikacji do użycia
- `MaxRating` Maksymalny dozwolony poziom klasyfikacji
- `EmptyStarCssClass` Klasa CSS do użycia podczas elementu klasyfikacji (gwiazdkę) jest pusty
- `FilledStarCssClass` Klasa CSS do użycia podczas wypełniania elementu klasyfikacji (gwiazdkę)
- `StarCssClass` Klasa CSS do użycia na potrzeby stat widoczne
- `WaitingStarCssClass` Klasa CSS do użycia podczas gwiazdki są wysyłane do serwera

A Oto kod znaczników, który tworzy kontrolkę klasyfikacji z pięciu elementów (smileys), których brak wypełniania początkowo:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

Teraz trzech klas CSS przywoływanego należy Pokaż pliki odpowiedni obraz, który jest łatwo zrobić przy użyciu CSS:

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

Upewnij się, że Podaj szerokość i wysokość trzy obrazy, w przeciwnym razie wyświetlenia może wyglądać nieco messed w górę.

Na koniec wynik Klasyfikacja powinien wyświetlane użytkownikowi (lub, co najmniej zapisane w bazie danych). Dlatego Dodaj etykietę dla danych wyjściowych wiadomości SMS i przycisk Prześlij post-back formularza klasyfikacji na serwerze:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

W kodzie po stronie serwera, dostępu formantu klasyfikacji za pośrednictwem jego `ID` i uzyskuje dostęp do jego `CurrentRating` właściwość, która jest liczba elementów wybranej klasyfikacji, w tym przykładzie wartość z zakresu od 0 do 5.

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

Strony i załadować go w przeglądarce. Po umieszczeniu na elementy klasyfikacji (początkowo puste), występuje efekt JavaScript: zmiany klasyfikacji. Po kliknięciu zestaw gwiazdek bieżąca ocena są zachowywane. Na koniec po przesłaniu formularza kod po stronie serwera generuje wybranej klasyfikacji.


[![Tworzenie system klasyfikacji z minimalnym kodu](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

Tworzenie system klasyfikacji z minimalnym kodu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](creating-a-rating-control-vb.md)
