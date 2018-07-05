---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animowanie kontrolki UpdatePanel (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Dla zawartości...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 5f14a3297c2f3ddd6b0817cc8e46ac23b3a06c0b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369719"
---
<a name="animating-an-updatepanel-control-vb"></a>Animowanie kontrolki UpdatePanel (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Dla zawartości UpdatePanel specjalne urządzenia extender istnieje silnie zależy od ram animacji: UpdatePanelAnimation. W tym samouczku pokazano, jak skonfigurować takie animacji dla kontrolki UpdatePanel.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Dla zawartości `UpdatePanel`, specjalne urządzenia extender istnieje która intensywnie korzysta z framework animacji: `UpdatePanelAnimation`. W tym samouczku pokazano, jak skonfigurować animacji dla `UpdatePanel`.

## <a name="steps"></a>Kroki

Pierwszym krokiem jest jak zwykle obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX jest ładowany i można go używać razem sterowania:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

Animacja w tym scenariuszu zostaną zastosowane do programu ASP.NET `Wizard` formantu sieci web znajdującej się w `UpdatePanel`. Trzy kroki (dowolną) zawierają wystarczającej liczby opcji, aby wyzwolić ogłaszania zwrotnego:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

Znaczniki, które są niezbędne do `UpdatePanelAnimationExtender` kontroli jest podobna do znaczników używany dla `AnimationExtender`. W `TargetControlID` atrybutu, firma Microsoft zapewnia `ID` z `UpdatePanel` animować; w ramach `UpdatePanelAnimationExtender` kontroli `<Animations>` element posiada znaczników XML dla animation(s). Jednak jest jedną różnicą: ilość zdarzenia i procedury obsługi zdarzeń jest ograniczona w porównaniu z `AnimationExtender`. Aby uzyskać `UpdatePanels`, tylko dwa z nich istnieje:

- `<OnUpdated>` Po zaktualizowaniu kontrolki UpdatePanel
- `<OnUpdating>` Po rozpoczęciu aktualizowania kontrolki UpdatePanel

W tym scenariuszu nowej zawartości `UpdatePanel` (po zwrotu) są zanikanie. Jest to niezbędne znaczników do tego:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Teraz przy każdym wystąpieniu ogłaszania zwrotnego w ramach kontrolki UpdatePanel, nowej zawartości panelu zanikanie.


[![Stopniowe następnego kroku w Kreatorze](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

Stopniowe następnego kroku w Kreatorze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-an-updatepanel-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](changing-an-animation-using-client-side-code-vb.md)
> [dalej](dynamically-controlling-updatepanel-animations-vb.md)
