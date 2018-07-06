---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Wyzwalanie animacji w innej kontrolce (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Ogólnie rzecz biorąc, uruchamianie...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a7eeb522ae982cd5a84aa9c8e4228871c8c78440
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810805"
---
<a name="triggering-an-animation-in-another-control-c"></a>Wyzwalanie animacji w innej kontrolce (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Ogólnie rzecz biorąc uruchamianie animacji jest wyzwalana przez interakcji użytkownika z tej samej kontrolki. Jest jednak również możliwość oddziałują na jeden formant, a następnie animacji innej kontrolki.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Ogólnie rzecz biorąc uruchamianie animacji jest wyzwalana przez interakcji użytkownika z tej samej kontrolki. Jest jednak również możliwość oddziałują na jeden formant, a następnie animacji innej kontrolki.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Aby rozpocząć, animowanie panelu, przycisk HTML jest używany. Należy pamiętać, że `<input type="button" />` jest uprzywilejowanym za pośrednictwem `<asp:Button />` ponieważ nie chcemy ogłaszania zwrotnego po kliknięciu tego przycisku przez użytkownika.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`. Jest ważne, aby ustawić `TargetControlID` identyfikator przycisku (element wyzwalanie animacji), nie identyfikatorowi panelu (elementu, jest animowany)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

W ramach `<Animations>` węzła, animacje miejsce w zwykły sposób. Aby stały się zmienić w panelu, ustaw nie przycisk `AnimationTarget` atrybutu dla każdego elementu animacji w ramach `AnimationExtender`. Wartość `AnimationTarget` oczywiście jest to identyfikator panelu. W ten sposób animacji się zdarzyć, za pomocą panelu, nie za pomocą przycisku wyzwalania. Oto `AnimationExtender` znaczników dla tego scenariusza:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Należy zwrócić uwagę specjalne kolejność, w jakiej są wyświetlane poszczególne animacji. Po pierwsze przycisk pobiera dezaktywowane, po uruchomieniu animacji. Ponieważ ma nie `AnimationTarget` atrybutu w `<EnableAction>` elementu, ta animacja jest stosowana do sterowania źródłowy: przycisku. Kroki opisane w dwóch następnych animacji są przeprowadzane parallelly (`<Parallel>` elementu). Zachowują się ich `AnimationTarget` Ustaw atrybuty `"Panel1"`, dlatego animowanie panelu nie przycisk.


[![Kliknięcie na przycisk uruchamiania animacji panelu](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Kliknięcie na przycisk uruchamia panel animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](disabling-actions-during-animation-cs.md)
> [dalej](modifying-animations-from-the-server-side-cs.md)
