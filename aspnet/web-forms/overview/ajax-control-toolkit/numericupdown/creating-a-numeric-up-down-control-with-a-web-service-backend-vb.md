---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Tworzenie liczbowe w górę/dół formantu z zaplecze usługi sieci Web (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Zamiast czekać na użytkownika, wpisz wartość w pole wyboru, liczbowe góra/dół formantu (znajdującego się w systemach Windows i innych systemów operacyjnych) może okazać się c w więcej...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 690fd89c552407ec5d77419aae2488e4832efe44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Tworzenie liczbowe formantu góra/dół z zaplecze usługi sieci Web (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Zamiast czekać na użytkownika, wpisz wartość w pole wyboru, liczbowe w górę/dół formantu (znajdującego się w systemach Windows i innymi systemami operacyjnymi) może okazać się za bardziej wygodne. Domyślnie numericupdown — formant zawsze zwiększa lub zmniejsza wartość 1, ale okaże się większą elastyczność i usługi sieci web.


## <a name="overview"></a>Omówienie

Zamiast czekać na użytkownika, wpisz wartość w pole wyboru, liczbowe w górę/dół formantu (znajdującego się w systemach Windows i innymi systemami operacyjnymi) może okazać się za bardziej wygodne. Domyślnie `NumericUpDown` formant zawsze zwiększa lub zmniejsza wartość 1, ale okaże się większą elastyczność i usługi sieci web.

## <a name="steps"></a>Kroki

Zawiera zestawie narzędzi programu ASP.NET AJAX kontroli `NumericUpDown` rozszerzeń, które automatycznie dodaje dwa przyciski do pola tekstowego: jeden dla zwiększenie jego wartości, jeden dla zmniejszenie go. Jednak formant obsługuje również wywołania usługi sieci web (lub wywołanie metody strony). Zawsze, gdy w górę lub w dół przycisk zostanie kliknięty, JavaScript, kod łączy się z serwerem sieci web i wykonuje metodę istnieje. Podpis metody jest następujące:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

`current` Argument jest bieżąca wartość w polu tekstowym; `tag` atrybut jest dodatkowy kontekst danych, które można ustawić jako właściwość `NumericUpDown` rozszerzeń (ale nie jest wymagane).

Dla tego przykładu liczbowe w górę/dół formantu tylko Zezwalaj wartości, które są potęgami liczby dwa: 1, 2, 4, 8, 16, 32, 64 i tak dalej. W związku z tym metoda wykonywać, gdy użytkownik chce zwiększyć wartość musi dokładnie stara wartość; inne metody Podziel wartość przez dwa. Dlatego jest tutaj usługę sieci web zakończenie:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Na koniec Utwórz nową stronę ASP.NET. Jak zwykle należy `ScriptManager` kontroli, `TextBox` kontroli i `NumericUpDownExtender` kontroli. W przypadku drugiego nagłówka musisz podać informacje o usłudze sieci web:

- `ServiceDownMethod` Nazwa w dół sieci web — metoda lub strona — Metoda
- `ServiceDownPath` Ścieżka do usługi sieci web z dół metody usługi; pominąć, jeśli używana jest metoda strony
- `ServiceUpMethod` Nazwa pracy sieci web — metoda lub strona — Metoda
- `ServiceUpPath` Ścieżka do usługi sieci web z się metody usługi; pominąć, jeśli używana jest metoda strony

W tym miejscu jest pełny kod znaczników dla strony:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Po uruchomieniu strony, zwróć uwagę, jak wartość w polu tekstowym zawsze podwaja podczas kliknij przycisk górny, a po kliknięciu przycisku niższe, jest dwukrotnie mniejsza.


[![Są wyświetlane tylko cyfry, które są potęgami liczby 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Są wyświetlane tylko cyfry, które są potęgami liczby 2 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
