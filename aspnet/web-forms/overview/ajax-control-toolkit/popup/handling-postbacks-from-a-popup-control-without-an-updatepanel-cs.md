---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: "Obsługa ogłaszania zwrotnego przez formant okna podręcznego bez elementu UpdatePanel (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Podczas odświeżania strony występuje w su..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Obsługa ogłaszania zwrotnego przez formant okna podręcznego bez elementu UpdatePanel (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Podczas odświeżania strony odbywa się w takich panelu i na stronie istnieje kilka paneli trudno jest określić, który panel został kliknięty.


## <a name="overview"></a>Omówienie

Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Podczas odświeżania strony odbywa się w takich panelu i na stronie istnieje kilka paneli trudno jest określić, który panel został kliknięty.

## <a name="steps"></a>Kroki

Korzystając z `PopupControl` odświeżenie strony, ale bez konieczności `UpdatePanel` na stronie Toolkit formantu nie oferuje możliwość określenia, który element klienta wyzwolił menu podręczne, które z kolei spowodowało ogłaszania zwrotnego. Jednak małych lewy dostarcza obejście dla tego scenariusza.

Po pierwsze, w tym miejscu jest podstawowa konfiguracja: dwa pola tekstowe podlegających zarówno samego menu podręczne kalendarza. Dwa `PopupControlExtenders` zebranie okna podręczne oraz pól tekstowych.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

Podstawowe rozwiązaniem jest dodanie ukryte pole formularza w &lt; `form` &gt; element, który zawiera pole tekstowe, która została uruchomiona menu podręcznego:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Podczas ładowania strony kodu JavaScript dodaje program obsługi zdarzeń do obu polach tekstowych: gdy użytkownik kliknie w polu tekstowym, jego nazwa jest zapisywany w ukryte pole formularza:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

W kodzie po stronie serwera można odczytać wartości pola ukrytego. Ponieważ ukryte pola są prosta do manipulowania, podejście listy dozwolonych adresów IP można sprawdzić poprawności wartości hidden jest wymagana. Po zidentyfikowaniu pola tekstowego poprawną datę z kalendarza są zapisywane w nim.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![Kliknij datę umieszcza je w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Kliknij datę umieszcza je w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

>[!div class="step-by-step"]
[Poprzednie](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[dalej](using-multiple-popup-controls-vb.md)
