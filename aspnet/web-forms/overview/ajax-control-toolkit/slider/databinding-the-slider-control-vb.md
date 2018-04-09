---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Wiązania danych formantu suwaka (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość powiązania bieżące położenie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ecd8598cd7fdcbbb4812e501bb30fa1f563a054
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="databinding-the-slider-control-vb"></a>Wiązania danych formantu suwaka (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość powiązania bieżącej pozycji suwaka inny formant ASP.NET.


## <a name="overview"></a>Omówienie

Kontrolka suwaka w zestawie narzędzi kontroli AJAX zapewnia graficznego suwaka, które można zmieniać za pomocą myszy. Istnieje możliwość powiązania bieżącej pozycji suwaka inny formant ASP.NET.

## <a name="steps"></a>Kroki

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

Następnie dodaj dwie `TextBox` formantów strony. Jeden zostanie on przekształcony w graficznym suwaka, a jeden z nich będą przechowywane pozycji suwaka.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

Następnym krokiem jest już ostatnim kroku. `SliderExtender` Formantu z zestawu ASP.NET AJAX kontroli narzędzi powoduje, że suwak poza pierwsze pole tekstowe i automatycznie aktualizuje drugiego pola tekstowego podczas zmiany pozycji suwaka. Do pracy, aby `SliderExtender`w `TargetControlID` atrybut musi być ustawiony na identyfikator pierwszego pola tekstowego; `BoundControlID` atrybut musi być ustawiony na identyfikator drugiego pola tekstowego.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Jak widać w przeglądarce, powiązania danych działa w obu kierunkach: wprowadzanie nową wartość w polu tekstowym aktualizuje pozycja suwaka. Jeśli wprowadzisz drugie pole tekstowe tylko do odczytu, można dodać ochrony słabe do pola tekstowego tak, aby przeszkodę dla użytkownika ręcznie zaktualizować wartości w nim.


[![Suwak i pole tekstowe są zsynchronizowane](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Suwak i pole tekstowe są zsynchronizowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-the-slider-control-with-auto-postback-vb.md)
