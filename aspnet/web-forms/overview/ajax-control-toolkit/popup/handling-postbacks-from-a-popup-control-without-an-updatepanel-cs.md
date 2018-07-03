---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Obsługa ogłaszania zwrotnego w kontrolce Popup bez kontrolki UpdatePanel (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Jeśli odświeżenie strony występuje w su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: d74a44a277bdcb460dc20b78bad3e5ed68445b4a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370986"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Obsługa ogłaszania zwrotnego w kontrolce Popup bez kontrolki UpdatePanel (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Podczas ogłaszania zwrotnego odbywa się w taki zespół wiąże się z kilku panelach na stronie trudno jest określić, który panel został kliknięty.


## <a name="overview"></a>Omówienie

Urządzenia extender PopupControl zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób wyzwalania okna podręcznego, po aktywowaniu wszelkimi innymi kontrolkami. Podczas ogłaszania zwrotnego odbywa się w taki zespół wiąże się z kilku panelach na stronie trudno jest określić, który panel został kliknięty.

## <a name="steps"></a>Kroki

Korzystając z `PopupControl` odświeżenie strony, ale bez konieczności `UpdatePanel` na stronie Toolkit formantu nie oferuje możliwość określenia, który element klienta wyzwolił okna podręcznego, który z kolei spowodował zwrotu. Jednak małych lewy dostarcza obejście dla tego scenariusza.

Po pierwsze, w tym miejscu jest konfiguracja podstawowa: dwóch pól tekstowych, które wyzwolić tego samego okna podręcznego kalendarza. Dwa `PopupControlExtenders` łączą ze sobą pola tekstowe i menu podręczne.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

Podstawowa koncepcja jest dodanie ukryte pole formularza w &lt; `form` &gt; element, który posiada pola tekstowego, która została uruchomiona, okno podręczne:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Po załadowaniu strony kodu w języku JavaScript dodaje procedurę obsługi zdarzeń do obu polach tekstowych: zawsze, gdy użytkownik kliknie pole tekstowe, jego nazwa zostanie zapisane ukryte pole formularza:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

W kodzie po stronie serwera można odczytać wartości pola ukrytego. Ponieważ ukryte pola są proste do manipulowania, lista dozwolonych sposobem sprawdzenia ukryte wartości jest wymagana. Po zidentyfikowaniu pole tekstowe poprawną datę z kalendarza są zapisywane do niego.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Kalendarz jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![Klikając pozycję na wartość typu date umieszcza go w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Klikając pozycję na wartość typu date umieszcza go w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [dalej](using-multiple-popup-controls-vb.md)
