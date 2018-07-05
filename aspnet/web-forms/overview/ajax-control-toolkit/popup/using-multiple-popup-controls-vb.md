---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Używanie wielu kontrolek Popup (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Istnieje również możliwość użycia m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: accb9134cc355e0c042114873973bcbfe9a4a0a4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384564"
---
<a name="using-multiple-popup-controls-vb"></a>Używanie wielu kontrolek Popup (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Istnieje również możliwość użycia więcej niż jeden formant okna podręcznego na jednej stronie.


## <a name="overview"></a>Omówienie

Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Istnieje również możliwość użycia więcej niż jeden formant okna podręcznego na jednej stronie.

## <a name="steps"></a>Kroki

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Następnie dodaj panel, który służy jako menu podręcznego. W tym scenariuszu zawiera panelu `Calendar` kontroli. Aby uniknąć odświeżenie strony, spowodowane ogłaszania zwrotnego kalendarza, panel jest umieszczany w ramach `UpdatePanel` sterowania:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

Strona zawiera również dwóch pól tekstowych. Dla każdego pola tekstowego podręcznego kalendarza są wyświetlane po aktywowaniu pola tekstowego.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Teraz rozszerzone każdego z dwóch pól tekstowych z `PopupControlExtender`. `TargetControlID` Atrybut zawiera identyfikator kontrolki powiązane z urządzenia extender. `PopupControlID` Atrybut zawiera identyfikator panel menu podręczne. W takim przypadku zarówno rozszerzeń Pokaż panel ten sam, ale różne zespoły są również możliwe.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Zawsze, gdy klikniesz pozycję w polu tekstowym, kalendarz pojawi się poniżej pola, umożliwiając wybranie daty. (Powrót wybranej daty w polach tekstowych zostały omówione w samouczku różnych.)


[![Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [dalej](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
