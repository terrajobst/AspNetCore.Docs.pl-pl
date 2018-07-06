---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Tworzenie kontrolki Rating (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności, oferują użytkownikom na szybkość artykuły lub elementów. Zwykle wymaga to pewnych wysiłków związanych z kodowaniem, ale mamy...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 67c66d2a28a247493a0b1ea67e15935c27af80ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830957"
---
<a name="creating-a-rating-control-c"></a>Tworzenie kontrolki Rating (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności, oferują użytkownikom na szybkość artykuły lub elementów. Zwykle wymaga to pewnych wysiłków związanych z kodowaniem, ale musimy Toolkit kontroli naszych usuwania.


## <a name="overview"></a>Omówienie

Wiele witryn sieci Web z handlu elektronicznego do witryn społeczności, oferują użytkownikom na szybkość artykuły lub elementów. Zwykle wymaga to pewnych wysiłków związanych z kodowaniem, ale musimy Toolkit kontroli naszych usuwania.

## <a name="steps"></a>Kroki

Przede wszystkim należy (co najmniej) dwa rodzaje obrazy: jeden dla wypełnienia elementu ocenę i jednego elementu pustego klasyfikacji. Element Klasyfikacja jest zazwyczaj gwiazdka lub uśmiechniętą. W tym scenariuszu możesz znaleźć trzy pliki, smiley.png i empty.png i done.png uśmiech jako część pobierania kodu źródłowego w ramach tego samouczka.

Następnie utwórz nowy plik platformy ASP.NET i Rozpocznij od dodania `ScriptManager` do niej kontrolkę:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

Następnie należy dodać `Rating` formantu z ASP.NET AJAX Control Toolkit. Następujące atrybuty muszą zostać ustawione dla tego przykładu:

- `CurrentRating` początkowa klasyfikacja ma być używany
- `MaxRating` Maksymalny poziom klasyfikacji
- `EmptyStarCssClass` Klasa CSS do użycia podczas element ranking (Star czyli rejestr) jest pusty
- `FilledStarCssClass` Klasa CSS do użycia podczas wypełniania elementu klasyfikacji (star)
- `StarCssClass` Klasa CSS na potrzeby stat widoczne
- `WaitingStarCssClass` Klasa CSS do użycia podczas oceny w formie gwiazdek jest wysyłane z powrotem do serwera

A Oto znaczników, który tworzy kontrolki rating z pięciu elementów (smileys), których brak jest wypełniane początkowo:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

Trzy klasy CSS do którego istnieje odwołanie teraz chcesz pokazać pliki odpowiedni obraz, który jest łatwo zrobić przy użyciu CSS:

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

Upewnij się, że podajesz, szerokość i wysokość trzy obrazy, w przeciwnym razie wyświetlanie mogą wyglądać nieco messed w górę.

Na koniec wynik klasyfikacji powinny być wyświetlane użytkownikowi (lub co najmniej zapisane w bazie danych). Dlatego należy dodać etykietę dla danych wyjściowych, wiadomości SMS i przycisk Prześlij, aby ponownie post formularza klasyfikacji do serwera:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

W kodzie po stronie serwera, dostępu do kontrolki oceny za pośrednictwem jego `ID` i uzyskuje dostęp do jego `CurrentRating` właściwość, która jest liczbą elementów wybranych klasyfikacji, w tym przykładzie wartość z zakresu od 0 do 5.

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

Zapisz stronę i załadować je bezpośrednio w przeglądarce. Po najechaniu kursorem na elementy klasyfikacji (początkowo pusta), efekt JavaScript pojawia się: zmiany klasyfikacji. Po kliknięciu zestaw gwiazdek bieżąca ocena są zachowywane. Na koniec po przesłaniu formularza danych wyjściowych wybranej klasyfikacji kodu po stronie serwera.


[![Tworzenie systemu klasyfikacji za pomocą minimalnej ilości kodu](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

Tworzenie systemu klasyfikacji za pomocą minimalnej ilości kodu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](creating-a-rating-control-vb.md)
