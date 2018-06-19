---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Zmiana animacji przy użyciu kodu po stronie klienta (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Animacji można również...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f572efeb012d88ab15740bab7b0e4383572f3f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870651"
---
<a name="changing-an-animation-using-client-side-code-c"></a>Zmiana animacji przy użyciu kodu po stronie klienta (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Animacji można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Animacji można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

Rzeczywiste animacji jest uruchamiany po kliknięciu przycisku kodu HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Należy pamiętać, że ma nie `<Animations>` węzła w `AnimationExtender` formantu. Niestandardowy kod JavaScript służy do zapewnienia animacji można używać z formantem.

Za pomocą interfejsu API serwera z `AnimationExtender`, nie istnieje łatwy sposób można przypisać animacji jeszcze do rozszerzeń. Jednak extender ujawnia kilka metod do odczytu i zapisu animacji zarejestrowany z różnych zdarzeń (`OnClick`, `OnLoad`i tak dalej). Oto kilka przykładów:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Format wartość zwracaną `get_*()` funkcje i format argumentu dla `set_*()` funkcji jest ciąg JSON, zawierający reprezentację obiektu jakie byłyby znaczników XML. Obecnie nie istnieje sposób do przekazania obiektu, ale istnieje możliwość odczytania obiektu z danym animacji (`get_OnXXXBehavior()` metody).

Oto ciągu JSON (bez cudzysłowów rozdzielające i dobrze sformatowana) reprezentujący animacji wyzwalane przez naciśnięcie przycisku, ale animacji panelu go i wygaszanie jednocześnie:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Następujący kod JavaScript przypisuje JSON descripting do `OnClick` animacji bieżącego rozszerzeń i uruchamia go:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![Animacja zostanie uruchomiona natychmiast, bez kliknięcia myszą (i z bardzo mało znaczników)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

Animacja zostanie uruchomiona natychmiast, bez kliknięcia myszą (i z bardzo mało znaczników) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-animations-using-client-side-code-cs.md)
> [dalej](animating-an-updatepanel-control-cs.md)
