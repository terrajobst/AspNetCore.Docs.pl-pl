---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Zmienianie animacji przy użyciu kodu po stronie klienta (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można również...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 9711e93ee6f119ec1825e32bdad64535435970c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808959"
---
<a name="changing-an-animation-using-client-side-code-c"></a>Zmienianie animacji przy użyciu kodu po stronie klienta (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

Rzeczywiste animacji jest uruchamiane po kliknięciu przycisku kodu HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Należy pamiętać, że ma nie `<Animations>` węzła w ramach `AnimationExtender` kontroli. Niestandardowy kod JavaScript służy do zapewnienia animacji, który ma być używany z formantem.

Podobnie jak w przypadku interfejsu API serwera z `AnimationExtender`, nie istnieje łatwy sposób jeszcze przypisać animacji do urządzenia extender. Jednak urządzenia extender udostępnia kilka metod do odczytu i zapisu animacji zarejestrowane przy użyciu różnych zdarzeń (`OnClick`, `OnLoad`i tak dalej). Oto kilka przykładów:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Format wartości zwracanej z `get_*()` funkcje i format argumentu dla `set_*()` funkcje jest ciągiem JSON, zapewniając znaczników XML, co będzie reprezentacja obiektu. Obecnie nie istnieje sposób do przekazania obiektu w, ale istnieje możliwość odczytania obiektu z danym animacji (`get_OnXXXBehavior()` metody).

Oto ciągu JSON (bez cudzysłowu ograniczająca i dobrze sformatowane) reprezentujący animacji wyzwalane po kliknięciu przycisku, ale animowanie panelu, zmienianie jej rozmiaru i wygaszanie w tym samym czasie:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Następujący kod JavaScript przypisuje JSON descripting do `OnClick` animacji bieżącego urządzenia extender i uruchamia go:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![Animacja jest uruchamiany natychmiast, bez kliknięcia (i z niewielkim znaczników)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

Animacja jest uruchamiany natychmiast, bez kliknięcie myszą (i z niewielkim znaczników) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-animations-using-client-side-code-cs.md)
> [dalej](animating-an-updatepanel-control-cs.md)
