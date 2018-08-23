---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Powiązanie danych kontrolki Slider (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy. Istnieje możliwość powiązania bieżące położenie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 284a650d1b63ceed887deb3ddd639fe9fd27019f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753925"
---
<a name="databinding-the-slider-control-vb"></a>Powiązanie danych kontrolki Slider (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy. Istnieje możliwość powiązać bieżące położenie suwaka innej kontrolki ASP.NET.


## <a name="overview"></a>Omówienie

Kontrolka suwaka w zestawu narzędzi AJAX Control Toolkit oferuje graficzny suwaka, które mogą być kontrolowane za pomocą myszy. Istnieje możliwość powiązać bieżące położenie suwaka innej kontrolki ASP.NET.

## <a name="steps"></a>Kroki

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

Następnie dodaj dwa `TextBox` kontrolek do strony. Jeden zostanie przekształcony w graficznym suwaka, a drugi będzie przechowywać położenie suwaka.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

Następnym krokiem jest już ostatni krok. `SliderExtender` Formantu z ASP.NET AJAX Control Toolkit sprawia, że suwak poza pierwszym polu tekstowym i automatycznie aktualizuje drugie pole tekstowe, gdy suwak pozycji zmiany. W kolejności, w tym do pracy `SliderExtender`firmy `TargetControlID` atrybutu musi być ustawione na identyfikator pierwszego pola tekstowego; `BoundControlID` atrybutu musi być ustawione na identyfikator drugiego pola tekstowego.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Jak widać w przeglądarce, powiązanie danych działa w obu kierunkach: wprowadzając nową wartość w polu tekstowym aktualizuje położenie suwaka. Jeśli zostało zgłoszone drugie pole tekstowe tylko do odczytu, można dodać słabe ochrony do pola tekstowego, aby jest trudniejsze dla użytkownika ręcznie zaktualizować wartość w miejscu.


[![Suwaka i pole tekstowe są zsynchronizowane](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Suwaka i pole tekstowe są zsynchronizowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-the-slider-control-with-auto-postback-vb.md)
