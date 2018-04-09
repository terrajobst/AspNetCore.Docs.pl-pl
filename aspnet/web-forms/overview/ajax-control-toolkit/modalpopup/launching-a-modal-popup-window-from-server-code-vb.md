---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Uruchamianie okna podręcznego modalne z kodu serwera (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Jednakże w niektórych scenariuszach wymagane tego t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 46554dae60ad9cd13e97e5755e95cb2125d1fed9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>Uruchamianie okna podręcznego modalne z kodu serwera (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Jednak w niektórych scenariuszach wymagane, że otwarcie modalnych menu podręczne jest wyzwalane po stronie serwera.


## <a name="overview"></a>Omówienie

Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Jednak w niektórych scenariuszach wymagane, że otwarcie modalnych menu podręczne jest wyzwalane po stronie serwera.

## <a name="steps"></a>Kroki

Po pierwsze kontrolkę przycisku ASP.NET sieci web jest wymagany do pokazują, jak działa Kontrola ModalPopup. Dodawanie przycisku w &lt;formularza&gt; element na nową stronę:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Następnie należy kod znaczników dla elementu popup, który chcesz utworzyć. Zdefiniuj go jako `<asp:Panel>` kontroli i upewnij się, że zawiera on kontrolkę przycisku. Formant ModalPopup oferuje funkcje dokonanie takiego przycisku Zamknij podręcznego; w przeciwnym razie jest prosty sposób pozwól mu znikają.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Następnie dodaj formant ModalPopup w zestawie narzędzi programu ASP.NET AJAX do strony. Ustawianie właściwości przycisku, który jest ładowany formant, przycisk, dzięki czemu znikają i identyfikator rzeczywiste menu podręczne.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

W przypadku wszystkich stron sieci web oparta na programie ASP.NET AJAX; Menedżer skryptów jest wymagane do załadowania wymagane biblioteki JavaScript w przypadku przeglądarek inny element docelowy:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Można uruchomić przykład w przeglądarce. Po kliknięciu przycisku pojawi się modalne okno podręczne. Aby osiągnąć ten sam efekt przy użyciu kodu po stronie serwera, wymagany jest przycisk Nowy:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Jak widać, kliknij przycisk odświeżania strony generuje i wykonuje `ServerButton_Click()` metody na serwerze. W przypadku tej metody wywołuje funkcję JavaScript `launchModal()` wykonaniu funkcji JavaScript należy dokładnie, będą wykonywane po załadowaniu strony:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

Zadaniem `launchModal()` są wyświetlane ModalPopup. `launchModal()` Funkcja jest wykonywana po załadowaniu pełną strony HTML. W tej chwili jednak ASP.NET AJAX framework nie został całkowicie załadowany jeszcze. W związku z tym `launchModal()` funkcja po prostu ustawia zmienną kontroli ModalPopup musi być wyświetlana w późniejszym czasie na:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

`pageLoad()` Funkcji JavaScript jest specjalnych funkcji, który jest wykonywany po całkowitym załadowaniem ASP.NET AJAX. W związku z tym dodamy kod do tej funkcji, aby pokazać ModalPopup sterowania, ale tylko wtedy, gdy `launchModal()` została wywołana przed:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

`$find()` Funkcji szuka nazwanego elementu na stronie i oczekuje identyfikator po stronie serwera jako parametr. W związku z tym `$find("mpe")` zwraca reprezentację klienta formantu ModalPopup; `show()` metoda umożliwia podręcznego są wyświetlane.


[![Modalnych menu podręczne jest wyświetlane, gdy zostanie kliknięty albo przycisków](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

Modalnych menu podręczne jest wyświetlane, gdy zostanie kliknięty albo przycisków ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](positioning-a-modalpopup-cs.md)
> [dalej](using-modalpopup-with-a-repeater-control-vb.md)
