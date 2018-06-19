---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animowanie formantu UpdatePanel (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Zawartości...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c1114b74fd152a4ea85aa10850860f75573adee
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873160"
---
<a name="animating-an-updatepanel-control-vb"></a>Animowanie formantu UpdatePanel (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Zawartość elementu UpdatePanel, rozszerzający specjalne umożliwiającą odgrywa framework animacji: UpdatePanelAnimation. Ten samouczek pokazuje, jak skonfigurować takie animacji dla elementu UpdatePanel.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Zawartości `UpdatePanel`, rozszerzający specjalne umożliwiającą odgrywa framework animacji: `UpdatePanelAnimation`. W tym samouczku przedstawiono sposób ustawiania animacji dla `UpdatePanel`.

## <a name="steps"></a>Kroki

Pierwszym krokiem jest normalnie obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX została załadowana i można go używać zestawu narzędzi kontroli:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

Animacja w tym scenariuszu zostaną zastosowane dla platformy ASP.NET `Wizard` kontrolka sieci web znajdującej się w `UpdatePanel`. Trzy kroki (dowolną) umożliwiają za mało wyzwolenia ogłaszania zwrotnego:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

Kod znaczników, które są niezbędne do `UpdatePanelAnimationExtender` formant jest podobna do znacznika używany do `AnimationExtender`. W `TargetControlID` atrybutu udostępniamy `ID` z `UpdatePanel` do animowania; w `UpdatePanelAnimationExtender` kontroli, `<Animations>` element posiada znaczników XML dla animacje. Jednak nie ma różnicy jednego: ilość zdarzenia i procedury obsługi zdarzeń jest ograniczona w porównaniu z `AnimationExtender`. Aby uzyskać `UpdatePanels`, tylko dwa z nich istnieje:

- `<OnUpdated>` Po zaktualizowaniu UpdatePanel
- `<OnUpdating>` Po uruchomieniu aktualizacji UpdatePanel

W tym scenariuszu nowej zawartości `UpdatePanel` (po odświeżenie strony) są zanikania. Jest to niezbędne znacznika, w tym:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Teraz po zmianie odświeżania strony w elemencie UpdatePanel, nowej zawartości panelu zanikania sprawnie.


[![Następny krok kreatora zanikania jest](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

Następny krok kreatora zanikania jest ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-an-updatepanel-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](changing-an-animation-using-client-side-code-vb.md)
> [dalej](dynamically-controlling-updatepanel-animations-vb.md)
