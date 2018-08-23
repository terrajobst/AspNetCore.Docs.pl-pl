---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Zmienianie animacji przy użyciu kodu po stronie klienta (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można również...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: bfce413cd45daab62a247d596d9b51cb82cc7f97
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753300"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>Zmienianie animacji przy użyciu kodu po stronie klienta (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

Rzeczywiste animacji jest uruchamiane po kliknięciu przycisku kodu HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Należy pamiętać, że ma nie `<Animations>` węzła w ramach `AnimationExtender` kontroli. Niestandardowy kod JavaScript służy do zapewnienia animacji, który ma być używany z formantem.

Podobnie jak w przypadku interfejsu API serwera z `AnimationExtender`, nie istnieje łatwy sposób jeszcze przypisać animacji do urządzenia extender. Jednak urządzenia extender udostępnia kilka metod do odczytu i zapisu animacji zarejestrowane przy użyciu różnych zdarzeń (`OnClick`, `OnLoad`i tak dalej). Oto kilka przykładów:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Format wartości zwracanej z `get_*()` funkcje i format argumentu dla `set_*()` funkcje jest ciągiem JSON, zapewniając znaczników XML, co będzie reprezentacja obiektu. Obecnie nie istnieje sposób do przekazania obiektu w, ale istnieje możliwość odczytania obiektu z danym animacji (`get_OnXXXBehavior()` metody).

Oto ciągu JSON (bez cudzysłowu ograniczająca i dobrze sformatowane) reprezentujący animacji wyzwalane po kliknięciu przycisku, ale animowanie panelu, zmienianie jej rozmiaru i wygaszanie w tym samym czasie:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

Następujący kod JavaScript przypisuje JSON descripting do `OnClick` animacji bieżącego urządzenia extender i uruchamia go:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![Animacja jest uruchamiany natychmiast, bez kliknięcia (i z niewielkim znaczników)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

Animacja jest uruchamiany natychmiast, bez kliknięcie myszą (i z niewielkim znaczników) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-animations-using-client-side-code-vb.md)
> [dalej](animating-an-updatepanel-control-vb.md)
