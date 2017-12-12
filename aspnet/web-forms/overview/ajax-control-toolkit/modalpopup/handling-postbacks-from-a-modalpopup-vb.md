---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: "Obsługa ogłaszania zwrotnego z ModalPopup (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Specjalne należy zachować ostrożność podczas pos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 7915d555fef363f41aa51bd77f0183c97c2f3769
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a>Obsługa ogłaszania zwrotnego z ModalPopup (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Specjalne należy uważać podczas odświeżania strony na podstawie w podręcznym.


## <a name="overview"></a>Omówienie

Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Specjalne należy uważać podczas odświeżania strony na podstawie w podręcznym.

## <a name="steps"></a>Kroki

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Następnie dodaj panel, która służy jako modalnych menu podręczne. Użytkownik może wprowadzić nazwę i adres e-mail. Przycisk służy do menu podręcznego. Zamknij i Zapisz informacje. Należy pamiętać, że `OnClick` atrybutu jest ustawiona, dzięki czemu odświeżania strony występuje po kliknięciu tego przycisku:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

Sama strona zawiera dwie etykiety dla tych samych informacji: Nazwa i adres e-mail. Przycisk służy do wyzwalania modalnych menu podręczne:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Aby wprowadzić podręcznego są wyświetlane, Dodaj `ModalPopupExtender` formantu. Ustaw `PopupControlID` atrybutu ID panelu i `TargetControlID` przycisku ID:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Teraz przy każdym `Save` kliknięciu przycisku w podręcznym modalnej po stronie serwera `SaveData()` metoda jest wykonywana. Istnieje można zapisać wprowadzonych danych w magazynie danych. Dla uproszczenia nowych danych jest tylko dane wyjściowe w etykiecie:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Ponadto kontrolki textbox w podręcznym modalne powinno być wypełnione z bieżącym nazwę i adres e-mail. Jednak jest to konieczne tylko po wystąpieniu nie ogłaszania zwrotnego. W przypadku odświeżania strony, funkcja viewstate programu ASP.NET spowoduje automatyczne wypełnienie pola tekstowe z odpowiednimi wartościami.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![Modalne podręcznego powoduje odświeżenie strony](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

Modalne podręcznego powoduje odświeżenie strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

>[!div class="step-by-step"]
[Poprzednie](using-modalpopup-with-a-repeater-control-vb.md)
[dalej](positioning-a-modalpopup-vb.md)
