---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Dodawanie animacji do kontrolki (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Ten samouczek pokazuje, jak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 80c982e041af2d0b9ee789665613ced0311dfcc9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396575"
---
<a name="adding-animation-to-a-control-c"></a>Dodawanie animacji do kontrolki (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. W tym samouczku pokazano, jak skonfigurować takie animacji.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. W tym samouczku pokazano, jak skonfigurować takie animacji.

## <a name="steps"></a>Kroki

Pierwszym krokiem jest jak zwykle obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX jest ładowany i można go używać razem sterowania:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

Animacji w tym scenariuszu zostaną zastosowane do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

Skojarzona klasa CSS w panelu definiuje kolor tła i szerokości:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Następnie up, potrzebujemy `AnimationExtender`. Po podaniu `ID` i zwykle `runat="server"`, `TargetControlID` atrybutu musi być równa kontrolki animacji w naszym przypadku panelu:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

Całe zastosowania animacji deklaratywne, przy użyciu składni XML, Niestety obecnie nie są w pełni obsługiwane przez funkcję IntelliSense programu Visual Studio. Węzeł główny jest `<Animations>;` w tym węźle kilka zdarzeń są dozwolone, które określają, kiedy animation(s) zakończeniu miejsce:

- `OnClick` (kliknięcie myszą)
- `OnHoverOut` (Jeśli kursora myszy poza formant)
- `OnHoverOver` (gdy wskaźnika myszy nad formant, zatrzymanie `OnHoverOut` animacji)
- `OnLoad` (po stronie został załadowany)
- `OnMouseOut` (Jeśli kursora myszy poza formant)
- `OnMouseOver` (gdy wskaźnika myszy nad formant, nie zatrzymywanie `OnMouseOut` animacji)

Struktura zawiera zestaw animacji, każdy z nich reprezentowany przez własną — element XML. Oto zaznaczenia:

- `<Color>` (zmiana koloru)
- `<FadeIn>` (rozjaśnianie)
- `<FadeOut>` (wygaszanie)
- `<Property>` (zmiana właściwości kontrolki)
- `<Pulse>` (pulsating)
- `<Resize>` (zmiana rozmiaru)
- `<Scale>` (proporcjonalnie zmiany rozmiaru)

W tym przykładzie panelu są zanikanie. Animacja podejmują 1,5 s (`Duration` atrybut), wyświetlanie 24 ramek (kroki animację) na sekundę (`Fps` attributs). Oto kompletny kod znaczników dla `AnimationExtender` sterowania:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Po uruchomieniu tego skryptu, panel jest wyświetlany i stopniowo zmniejsza się w ciągu półtora sekund.


[![Panel jest Ściemnianie](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Panel jest wygaszanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](executing-several-animations-at-the-same-time-cs.md)
