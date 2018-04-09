---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Pozycjonowanie ModalPopup (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Jednak formantu nie oferuje...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: bee5be84259231d8cd5efde74b610d72f5e250cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="positioning-a-modalpopup-c"></a>Pozycjonowanie ModalPopup (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Formant nie oferuje jednak wbudowanej funkcji do pozycji menu podręcznego.


## <a name="overview"></a>Omówienie

Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Formant nie oferuje jednak wbudowanej funkcji do pozycji menu podręcznego.

## <a name="steps"></a>Kroki

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager`. formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Następnie dodaj panel, która służy jako modalnych menu podręczne. Przycisk służy do zamykania menu podręcznego:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Zawsze, gdy podręczny jest wyświetlany, powinien zostać umieszczony w określone miejsce na stronie. Dla tego zadania jest tworzony funkcji JavaScript po stronie klienta. Najpierw próbuje uzyskać dostęp z panelu. Jeśli zakończy się powodzeniem, pozycji panel jest ustawiany za pomocą CSS i JavaScript (zmiana spowoduje pozycja podręcznego w). Jednak `ModalPopupExtender` kontroli próbuje również pozycji menu podręcznego. W związku z tym kod JavaScript wielokrotnie umieszcza menu podręczne, co dziesiątym dniu sekundy.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Jak widać, zwracana wartość `setTimeout()` metody JavaScript jest zapisywany w zmiennej globalnej. Dzięki temu można zatrzymać powtarzane pozycjonowanie podręcznego na żądanie, używając `clearTimeout()` metody:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

Teraz pozostało celu jest ustawienie przeglądarki wywoływać te funkcje, w miarę potrzeb. `movePanel()` Można wywołać funkcji JavaScript, po kliknięciu przycisku wyzwala panelu:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

I `stopMoving()` funkcja wejścia play po zamknięciu menu podręcznego. może to być wyzwalane w `ModalPopupExtender` sterowania:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


[![W pozycji wyznaczonych pojawi się modalne okno podręczne](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

Modalne wyskakującego na wyznaczonych pozycji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](handling-postbacks-from-a-modalpopup-cs.md)
> [dalej](launching-a-modal-popup-window-from-server-code-vb.md)
