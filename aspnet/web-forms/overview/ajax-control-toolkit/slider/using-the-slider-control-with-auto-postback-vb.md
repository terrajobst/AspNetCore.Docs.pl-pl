---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Używanie kontrolki Slider z automatycznym ogłaszaniem zwrotnym (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy. Jest możliwe automatycznie Księguj suwaka...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ad701763f5d391a793083a1d81db69e7f712069
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809858"
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>Używanie kontrolki Slider z automatycznym ogłaszaniem zwrotnym (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy. Istnieje możliwość zmiany autopostback suwaka po jego wartość.


## <a name="overview"></a>Omówienie

Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy. Istnieje możliwość zmiany autopostback suwaka po jego wartość.

## <a name="steps"></a>Kroki

Aby wprowadzić suwak automatycznie zwrotu na zmianę, obu polach tekstowych muszą atrybut `AutoPostBack="true"`: pole tekstowe, które staną się suwak i pola tekstowego, który przechowuje położenie suwaka. W tym miejscu jest wymagane w tym:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

`SliderExtender` Formantu z ASP.NET AJAX Control Toolkit przypisuje funkcji suwaka do dwóch pól tekstowych:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Element label dodatkowe posłuży później poinformowania użytkownika o odświeżenie strony:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Na koniec `ScriptManager` kontroli ASP.NET AJAX ładuje wymagany język JavaScript dla kontrolki zestawu narzędzi do pracy:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Teraz wrócili, publikuje suwaka po stronie serwera to zdarzenie może być przechwycony i podjęto jakieś działanie:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![Przesunięcie suwaka wyzwala ogłaszania zwrotnego](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Przesunięcie suwaka wyzwala ogłaszania zwrotnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![Później Data zmiany są zapisywane w etykiecie](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Później, Data zmiany są zapisywane w etykiecie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](databinding-the-slider-control-cs.md)
> [dalej](databinding-the-slider-control-vb.md)
