---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Używanie wielu kontrolek Popup (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Istnieje również możliwość użycia m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cf7f929b696240e6805a74fb576ad56738491fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755778"
---
<a name="using-multiple-popup-controls-c"></a>Używanie wielu kontrolek Popup (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Istnieje również możliwość użycia więcej niż jeden formant okna podręcznego na jednej stronie.


## <a name="overview"></a>Omówienie

Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Istnieje również możliwość użycia więcej niż jeden formant okna podręcznego na jednej stronie.

## <a name="steps"></a>Kroki

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

Następnie dodaj panel, który służy jako menu podręcznego. W tym scenariuszu zawiera panelu `Calendar` kontroli. Aby uniknąć odświeżenie strony, spowodowane ogłaszania zwrotnego kalendarza, panel jest umieszczany w ramach `UpdatePanel` sterowania:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

Strona zawiera również dwóch pól tekstowych. Dla każdego pola tekstowego podręcznego kalendarza są wyświetlane po aktywowaniu pola tekstowego.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Teraz rozszerzone każdego z dwóch pól tekstowych z `PopupControlExtender`. `TargetControlID` Atrybut zawiera identyfikator kontrolki powiązane z urządzenia extender. `PopupControlID` Atrybut zawiera identyfikator panel menu podręczne. W takim przypadku zarówno rozszerzeń Pokaż panel ten sam, ale różne zespoły są również możliwe.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Zawsze, gdy klikniesz pozycję w polu tekstowym, kalendarz pojawi się poniżej pola, umożliwiając wybranie daty. (Powrót wybranej daty w polach tekstowych zostały omówione w samouczku różnych.)


[![Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-multiple-popup-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
