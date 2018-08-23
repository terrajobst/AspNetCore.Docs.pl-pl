---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Tworzenie Numeric w górę/dół kontrolki z zapleczem usługi internetowej (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Zamiast umożliwienie użytkownikowi wpisz wartość w polu wyboru, kontrolki numeric up/down (znajdującą się na Windows i innych systemów operacyjnych) może okazać się c ponieważ coraz więcej...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: ffefed61e259994990315d17a545ef74074092a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752903"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Tworzenie kontrolki Numeric Up/Down z zapleczem usługi internetowej (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> Zamiast umożliwienie użytkownikowi wpisz wartość w polu wyboru, liczbowych w górę/dół kontroli (co oznacza, że istnieje na Windows i innych systemów operacyjnych) można udowodnić, że ponieważ coraz więcej doświadczenia. Domyślnie formant NumericUpDown zawsze zwiększa lub zmniejsza wartość 1, ale usługi sieci web okaże bardziej elastyczne.


## <a name="overview"></a>Omówienie

Zamiast umożliwienie użytkownikowi wpisz wartość w polu wyboru, liczbowych w górę/dół kontroli (co oznacza, że istnieje na Windows i innych systemów operacyjnych) można udowodnić, że ponieważ coraz więcej doświadczenia. Domyślnie `NumericUpDown` kontroli zawsze zwiększa lub zmniejsza wartość 1, ale usługi sieci web okaże bardziej elastyczne.

## <a name="steps"></a>Kroki

ASP.NET AJAX Control Toolkit zawiera `NumericUpDown` rozszerzeń, który automatycznie dodaje dwa przyciski do pola tekstowego: jedną zwiększenia jego wartość, jedną dla zmniejsza się go. Jednak formant obsługuje również wywołanie usługi sieci web (lub wywołanie metody strony). Zawsze, gdy w górę lub dół przycisk po kliknięciu JavaScript kod łączy się z serwerem sieci web i wykonuje metodę istnieje. Podpis metody jest następujące:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

`current` Argument jest bieżąca wartość w polu tekstowym; `tag` atrybut jest dodatkowy kontekst danych, którą można określić jako właściwość `NumericUpDown` rozszerzeń (ale nie jest wymagane).

W tym przykładzie liczbowe w górę/dół kontroli tylko Zezwalaj na wartości, które są potęgi liczb: 1, 2, 4, 8, 16, 32, 64 i tak dalej. W związku z tym metoda wykonania, gdy użytkownik chce, aby zwiększyć wartość należy double stara wartość; inna metoda należy podzielić wartość przez dwa. Oto więc usługę sieci web zakończenie:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Na koniec Utwórz nową stronę programu ASP.NET. Należy w zwykły sposób, `ScriptManager` kontroli `TextBox` kontroli i `NumericUpDownExtender` kontroli. W przypadku drugiego nagłówka musisz podać informacje o usłudze sieci web:

- `ServiceDownMethod` Nazwa w dół metodę sieci web lub strony — metoda
- `ServiceDownPath` Ścieżka do usługi sieci web za pomocą metody dół usługi; pominąć, jeśli używana jest metoda strony
- `ServiceUpMethod` Nazwa pracy metodę sieci web lub strony — metoda
- `ServiceUpPath` Ścieżka do usługi sieci web za pomocą metody pracy usługi; pominąć, jeśli używana jest metoda strony

Oto kompletny kod znaczników dla strony:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Po uruchomieniu strony, zwróć uwagę, jak wartość w polu tekstowym zawsze podwaja się po kliknięciu przycisk w prawym górnym i filtrach, po kliknięciu przycisku niższe.


[![Są wyświetlane tylko cyfry, które są wartością potęgi liczby 2](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Są wyświetlane tylko cyfry, które są wartością potęgi liczby 2 ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
