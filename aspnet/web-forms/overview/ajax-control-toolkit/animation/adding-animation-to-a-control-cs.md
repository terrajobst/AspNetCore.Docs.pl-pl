---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Dodawanie animacji do formantu (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Ten samouczek pokazuje, jak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ba122660045c3f5dd4b11f118df174a79de814a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-animation-to-a-control-c"></a>Dodawanie animacji do formantu (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Ten samouczek pokazuje, jak skonfigurować takie animacji.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Ten samouczek pokazuje, jak skonfigurować takie animacji.

## <a name="steps"></a>Kroki

Pierwszym krokiem jest normalnie obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX została załadowana i można go używać zestawu narzędzi kontroli:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

Animacja w tym scenariuszu zostaną zastosowane do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

Skojarzona klasa CSS w panelu definiuje kolor tła i szerokości:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Następnie up, potrzebujemy `AnimationExtender`. Po podaniu `ID` i zwykle `runat="server"`, `TargetControlID` atrybut musi mieć ustawioną formantu animacji, w tym przypadku panelu:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

Całe zastosowania animacji deklaratywnie, używając składni XML, Niestety obecnie nie są w pełni obsługiwane przez funkcję IntelliSense programu Visual Studio. Węzeł główny jest `<Animations>;` w tym węźle kilka zdarzeń mogą ustalić, kiedy animacje zakończeniu miejscu:

- `OnClick` (kliknij przycisk myszy)
- `OnHoverOut` (gdy mysz opuści formantu)
- `OnHoverOver` (gdy mysz znajduje się nad formantem, zatrzymywanie `OnHoverOut` animacji)
- `OnLoad` (po załadowaniu strony)
- `OnMouseOut` (gdy mysz opuści formantu)
- `OnMouseOver` (gdy mysz znajduje się nad formantem, nie zatrzymuje `OnMouseOut` animacji)

Platformę jest dostarczany z zestawem animacji, każdy z nich reprezentowany przez jego własnej — element XML. Oto zaznaczenia:

- `<Color>` (zmiana koloru)
- `<FadeIn>` (zanikania)
- `<FadeOut>` (wygaszanie)
- `<Property>` (zmiana właściwości formantu)
- `<Pulse>` (pulsating)
- `<Resize>` (zmiana rozmiaru)
- `<Scale>` (proporcjonalnie zmiany rozmiaru)

W tym przykładzie panelu są zanikania. Animacja podejmują 1,5 s (`Duration` atrybut), wyświetlanie 24 ramki (procedura animacji) na sekundę (`Fps` attributs). W tym miejscu jest pełny kod znaczników dla `AnimationExtender` sterowania:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Po uruchomieniu tego skryptu, panel jest wyświetlany i stopniowo zmniejsza się w jednym i pół sekundy.


[![Wygaszanie jest panelu](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Wygaszanie jest panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](executing-several-animations-at-the-same-time-cs.md)
