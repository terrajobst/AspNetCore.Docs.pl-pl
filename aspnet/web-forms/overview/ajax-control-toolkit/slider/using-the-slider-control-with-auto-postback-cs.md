---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Za pomocą suwaka z automatycznego odświeżania strony (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość automatycznie Księguj suwaka...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: e347d20c5c2ee48e6ed801e95459af6f0bcd2667
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879247"
---
<a name="using-the-slider-control-with-auto-postback-c"></a>Za pomocą suwaka z automatycznego odświeżania strony (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość zmiany autopostback suwaka po jego wartość.


## <a name="overview"></a>Omówienie

Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość zmiany autopostback suwaka po jego wartość.

## <a name="steps"></a>Kroki

W celu suwak automatycznego odświeżania strony na zmianę, obu polach tekstowych wymagają atrybutu `AutoPostBack="true"`: pola tekstowego, który ma zostać suwak sam i pola tekstowego, która przechowuje pozycji suwaka. Oto wymagane znacznika, w tym:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

`SliderExtender` Formantu w zestawie narzędzi programu ASP.NET AJAX kontroli przypisuje funkcji suwak w polach tekstowych:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Element label dodatkowe później będzie służyć do poinformowania użytkownika odświeżania strony:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Na koniec `ScriptManager` ładowany formant programu ASP.NET AJAX JavaScript wymagane dla kontroli zestawu narzędzi do pracy:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Teraz suwak jest przesyłanie ponownie; po stronie serwera to zdarzenie może być przechwycono i reagować:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![Przesunięcie suwaka wyzwala odświeżania strony](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Przesunięcie suwaka wyzwala odświeżania strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![Później Data zmiany są zapisywane w etykiecie](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Później, Data zmiany są zapisywane w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Next](databinding-the-slider-control-cs.md)
