---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Wyzwolenie animacji w inny formant (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Ogólnie rzecz biorąc, uruchamianie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e94046ca70607e37c1b5ef57d5cedef67a236b94
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="triggering-an-animation-in-another-control-c"></a>Wyzwolenie animacji w inny formant (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Ogólnie rzecz biorąc uruchomienie animacji zostanie wywołany przez użytkownika za pomocą formantu tego samego. Jest jednak również możliwość interakcji z formantami, a następnie animacji inny formant.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Ogólnie rzecz biorąc uruchomienie animacji zostanie wywołany przez użytkownika za pomocą formantu tego samego. Jest jednak również możliwość interakcji z formantami, a następnie animacji inny formant.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Aby rozpocząć animacji panelu, przycisk HTML jest używany. Należy pamiętać, że `<input type="button" />` jest uprzywilejowanym za pośrednictwem `<asp:Button />` ponieważ nie chcemy odświeżania strony po kliknięciu tego przycisku.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`. Ważne jest, aby ustawić `TargetControlID` identyfikator przycisku (element wyzwalania animacji), nie identyfikatorowi panelu (element animowanej)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

W ramach `<Animations>` węzła, animacje miejsce w zwykły sposób. Aby je zmienić panelu, ustaw nie przycisk `AnimationTarget` atrybutu dla każdego elementu animacji w `AnimationExtender`. Wartość `AnimationTarget` oczywiście jest Identyfikatorem panelu. W ten sposób animacji się tak zdarzyć przy użyciu panelu, nie z przyciskiem wyzwalająca. Oto `AnimationExtender` znaczników dla tego scenariusza:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Należy zwrócić uwagę specjalne kolejność poszczególnych animacji. Przede wszystkim przycisku pobiera dezaktywowane, po uruchomieniu animacji. Ponieważ nie `AnimationTarget` atrybutu w `<EnableAction>` elementu, to animacji jest stosowana do formantu źródłowego: przycisku. Animacja następne dwa kroki przeprowadza się parallelly (`<Parallel>` elementu). Mają ich `AnimationTarget` ustawić atrybuty `"Panel1"`, w związku z tym animacji panelu, a nie przycisk.


[![Kliknięcie przycisku powoduje uruchomienie animacji panelu](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Kliknięcie przycisku powoduje uruchomienie animacji panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](disabling-actions-during-animation-cs.md)
> [dalej](modifying-animations-from-the-server-side-cs.md)
