---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Obsługa ogłaszania zwrotnego z ModalPopup (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Specjalne należy zachować ostrożność podczas pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 183725db62ba8b4037f368ed9d87d5059e3f1bcb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873735"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>Obsługa ogłaszania zwrotnego z ModalPopup (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Specjalne należy uważać podczas odświeżania strony na podstawie w podręcznym.


## <a name="overview"></a>Omówienie

Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Specjalne należy uważać podczas odświeżania strony na podstawie w podręcznym.

## <a name="steps"></a>Kroki

W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Następnie dodaj panel, która służy jako modalnych menu podręczne. Użytkownik może wprowadzić nazwę i adres e-mail. Przycisk służy do menu podręcznego. Zamknij i Zapisz informacje. Należy pamiętać, że `OnClick` atrybutu jest ustawiona, dzięki czemu odświeżania strony występuje po kliknięciu tego przycisku:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

Sama strona zawiera dwie etykiety dla tych samych informacji: Nazwa i adres e-mail. Przycisk służy do wyzwalania modalnych menu podręczne:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Aby wprowadzić podręcznego są wyświetlane, Dodaj `ModalPopupExtender` formantu. Ustaw `PopupControlID` atrybutu ID panelu i `TargetControlID` przycisku ID:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Teraz przy każdym `Save` kliknięciu przycisku w podręcznym modalnej po stronie serwera `SaveData()` metoda jest wykonywana. Istnieje można zapisać wprowadzonych danych w magazynie danych. Dla uproszczenia nowych danych jest tylko dane wyjściowe w etykiecie:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Ponadto kontrolki textbox w podręcznym modalne powinno być wypełnione z bieżącym nazwę i adres e-mail. Jednak jest to konieczne tylko po wystąpieniu nie ogłaszania zwrotnego. W przypadku odświeżania strony, funkcja viewstate programu ASP.NET spowoduje automatyczne wypełnienie pola tekstowe z odpowiednimi wartościami.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![Modalne podręcznego powoduje odświeżenie strony](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

Modalne podręcznego powoduje odświeżenie strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-modalpopup-with-a-repeater-control-cs.md)
> [dalej](positioning-a-modalpopup-cs.md)
