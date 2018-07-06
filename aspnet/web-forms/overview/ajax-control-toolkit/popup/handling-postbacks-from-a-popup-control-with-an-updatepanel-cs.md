---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Obsługa ogłaszania zwrotnego w kontrolce Popup z kontrolką UpdatePanel (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Szczególną uwagę należy zwrócić...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9fdf2821e005bfcb691134bb6994c01fa72b7abd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815385"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Obsługa ogłaszania zwrotnego w kontrolce Popup z kontrolką UpdatePanel (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Szczególną uwagę należy podjąć w przypadku ogłaszania zwrotnego w obrębie okna podręcznego.


## <a name="overview"></a>Omówienie

Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Szczególną uwagę należy podjąć w przypadku ogłaszania zwrotnego w obrębie okna podręcznego.

## <a name="steps"></a>Kroki

Korzystając z `PopupControl` z zwrotu `UpdatePanel` może uniemożliwić odświeżania strony spowodowane zwrotu. Następujące znaczniki definiuje kilka ważnych elementów:

- A `ScriptManager` kontrolki ASP.NET AJAX Control Toolkit działa
- Dwa `TextBox` formanty, które wyzwolą oba okna podręcznego
- A `Panel` formant, który będzie służyć jako menu podręcznego.
- W panelu `Calendar` kontroli jest osadzony w ramach `UpdatePanel` kontroli
- Dwa `PopupControlExtender` mechanizmy przypisać panelu do pól tekstowych

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Należy pamiętać, że `OnSelectionChanged` atrybutu `Calendar` kontrolki jest ustawiona. Tak, gdy użytkownik wybierze datę w kalendarzu, występuje odświeżenie strony, a także metoda po stronie serwera `c1_SelectionChanged()` jest wykonywany. W ramach tej metody należy pobrać i zapisywane z powrotem do pola tekstowego bieżącą datę.

Ma następującą składnię, w tym: po pierwsze, serwer proxy dla obiektu `PopupControlExtender` na stronie musi zostać wygenerowany. ASP.NET AJAX Control Toolkit oferuje `GetProxyForCurrentPopup()` metody. Obiekt, Metoda ta zwraca obsługuje `Commit()` metodę, która wysyła wartość do formantu, który wyzwolił menu podręczne (nie formant który wyzwolił wywołanie metody!). Poniższy kod zawiera wybranej daty jako argument dla `Commit()` metody, powodując kod w celu zapisania wybranej daty z powrotem do pola tekstowego:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Teraz po każdym kliknięciu datę w kalendarzu wybrana data pojawia się w polu tekstowym skojarzone tworzenie kontrolki selektora daty, które aktualnie znajdują się na wiele witryn sieci Web.


[![Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![Klikając pozycję na wartość typu date umieszcza go w polu tekstowym](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Klikając pozycję na wartość typu date umieszcza go w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](using-multiple-popup-controls-cs.md)
> [dalej](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
