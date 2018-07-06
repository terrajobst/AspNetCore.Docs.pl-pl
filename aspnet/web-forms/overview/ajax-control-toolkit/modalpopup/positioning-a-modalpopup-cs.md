---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Pozycjonowanie kontrolki ModalPopup (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta. Jednak formant nie oferuje...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 4f94c0a473b88c0155847d700c48faa34b84c940
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807067"
---
<a name="positioning-a-modalpopup-c"></a>Pozycjonowanie kontrolki ModalPopup (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta. Jednak formant nie oferuje wbudowane funkcje, aby ustalić położenie wyskakującego okienka.


## <a name="overview"></a>Omówienie

Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta. Jednak formant nie oferuje wbudowane funkcje, aby ustalić położenie wyskakującego okienka.

## <a name="steps"></a>Kroki

Aby aktywować tę funkcję ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager`. Kontrolka musi znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Następnie dodaj panel, który służy jako modalnego okna podręcznego. Przycisk jest używany, aby zamknąć okno podręczne:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Zawsze, gdy zostanie wyświetlone okno podręczne, powinien zostać umieszczony w określone miejsce na stronie. Dla tego zadania zostanie utworzona funkcja języka JavaScript po stronie klienta. Najpierw próbuje uzyskać dostęp w panelu. Jeśli się powiedzie, położenie panelu można ustawić przy użyciu CSS i JavaScript (zmiana położenia okna podręcznego na będą). Jednak `ModalPopupExtender` kontroli próbuje również położenie wyskakującego okienka. W związku z tym kod JavaScript wielokrotnie umieszcza okno podręczne, co dziesiątej sekundy.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Jak widać, wartość zwracana przez `setTimeout()` metody JavaScript są zapisywane w zmiennej globalnej. Dzięki temu można zatrzymać powtarzanych położenia okna podręcznego na żądanie, używając `clearTimeout()` metody:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

Teraz pozostało celu jest zapewnienie przeglądarki wywołać te funkcje, zawsze, gdy jest to odpowiednie. `movePanel()` Musi zostać wywołana funkcja języka JavaScript, po kliknięciu przycisku wyzwala panelu:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

I `stopMoving()` funkcja właśnie po zamknięciu okna podręcznego może to nastąpić w `ModalPopupExtender` sterowania:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


[![Modalnego okna podręcznego, który pojawia się w wyznaczonym miejscu](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

Modalnego okna podręcznego, który pojawia się w wyznaczonym miejscu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](handling-postbacks-from-a-modalpopup-cs.md)
> [dalej](launching-a-modal-popup-window-from-server-code-vb.md)
