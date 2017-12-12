---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: "Za pomocą suwaka z automatycznego odświeżania strony (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość automatycznie Księguj suwaka..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: fedd3ff947c6f5d5d4d00791087e9fd825fdf9d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>Za pomocą suwaka z automatycznego odświeżania strony (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość zmiany autopostback suwaka po jego wartość.


## <a name="overview"></a>Omówienie

Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość zmiany autopostback suwaka po jego wartość.

## <a name="steps"></a>Kroki

W celu suwak automatycznego odświeżania strony na zmianę, obu polach tekstowych wymagają atrybutu `AutoPostBack="true"`: pola tekstowego, który ma zostać suwak sam i pola tekstowego, która przechowuje pozycji suwaka. Oto wymagane znacznika, w tym:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

`SliderExtender` Formantu w zestawie narzędzi programu ASP.NET AJAX kontroli przypisuje funkcji suwak w polach tekstowych:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Element label dodatkowe później będzie służyć do poinformowania użytkownika odświeżania strony:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Na koniec `ScriptManager` ładowany formant programu ASP.NET AJAX JavaScript wymagane dla kontroli zestawu narzędzi do pracy:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Teraz suwak jest przesyłanie ponownie; po stronie serwera to zdarzenie może być przechwycono i reagować:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![Przesunięcie suwaka wyzwala odświeżania strony](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Przesunięcie suwaka wyzwala odświeżania strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![Później Data zmiany są zapisywane w etykiecie](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Później, Data zmiany są zapisywane w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

>[!div class="step-by-step"]
[Poprzednie](databinding-the-slider-control-cs.md)
[dalej](databinding-the-slider-control-vb.md)
