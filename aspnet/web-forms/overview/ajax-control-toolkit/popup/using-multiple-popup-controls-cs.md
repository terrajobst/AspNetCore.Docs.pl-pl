---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: "Używanie wielu formantów podręcznego (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Istnieje również możliwość użycia m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: d8c9742bb39b655115cb1862ff6e867989e63665
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-c"></a>Używanie wielu formantów podręcznego (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Istnieje również możliwość używania więcej niż jeden formant menu podręczne na jednej stronie.


## <a name="overview"></a>Omówienie

Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Istnieje również możliwość używania więcej niż jeden formant menu podręczne na jednej stronie.

## <a name="steps"></a>Kroki

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

Następnie dodaj panel, która służy jako menu podręcznego. W tym scenariuszu zawiera panelu `Calendar` formantu. Aby uniknąć odświeżenie strony spowodowane ogłaszania zwrotnego kalendarza, panelu jest umieszczany w `UpdatePanel` sterowania:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

Strona zawiera również dwóch pól tekstowych. Dla każdego pola tekstowego kalendarza podręcznego są wyświetlane po uaktywnieniu pola tekstowego.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Teraz Rozszerz każdą z dwóch polach z `PopupControlExtender`. `TargetControlID` Atrybutu zawiera identyfikator formantu związana z rozszerzeń. `PopupControlID` Atrybutu zawiera identyfikator panel menu podręczne. W takim przypadku zarówno Extender Pokaż samym panelu, ale panele różne możliwe, są również.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Teraz przy każdym kliknięciu w polu tekstowym, kalendarz pojawia się poniżej pola, co pozwala na wybranie daty. (Odzyskać wybranej daty w polach tekstowych zostanie omówiona w samouczku różnych.)


[![Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-multiple-popup-controls-cs/_static/image3.png))

>[!div class="step-by-step"]
[Dalej](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
