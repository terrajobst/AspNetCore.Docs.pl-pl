---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Obsługa ogłaszania zwrotnego w kontrolce ModalPopup (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta. Specjalne należy zachować ostrożność podczas pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 2c5c3b573b62d779ab09caad22b0c0e3a6995634
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399071"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>Obsługa ogłaszania zwrotnego w kontrolce ModalPopup (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta. Specjalne należy uważać podczas tworzenia ogłaszania zwrotnego z w ramach menu podręcznego.


## <a name="overview"></a>Omówienie

Kontrolki ModalPopup na zestawu narzędzi AJAX Control Toolkit oferuje prosty sposób utworzyć modalnego okna podręcznego, za pomocą oznacza, że po stronie klienta. Specjalne należy uważać podczas tworzenia ogłaszania zwrotnego z w ramach menu podręcznego.

## <a name="steps"></a>Kroki

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Następnie dodaj panel, który służy jako modalnego okna podręcznego. Użytkownik może wprowadzić nazwę i adres e-mail. Przycisk służy do Zamknij okno podręczne i Zapisz informacje. Należy pamiętać, że `OnClick` atrybut jest ustawiony tak, aby ogłaszania zwrotnego występuje po kliknięciu tego przycisku:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

Sama strona składa się z dwóch etykiet dla dokładnie te same informacje: Nazwa i adres e-mail. Przycisk służy do wyzwalania modalnego okna podręcznego:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Aby było wyświetlane wyskakujące okienko, Dodaj `ModalPopupExtender` kontroli. Ustaw `PopupControlID` atrybutu ID panelu i `TargetControlID` identyfikatorowi przycisku:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Teraz zawsze, gdy `Save` w ramach modalnego okna podręcznego przycisku, po stronie serwera `SaveData()` metoda jest wykonywana. Można zapisać wprowadzonych danych w magazynie danych. Dla uproszczenia nowe dane po prostu znajdują się dane wyjściowe w etykiecie:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Ponadto kontrolki pola tekstowego, w ramach modalnego okna podręcznego powinny być widoczne bieżącą nazwę i adres e-mail. Jednak jest to konieczne tylko po wystąpieniu nie zwrotu. W przypadku zwrot funkcji viewstate programu ASP.NET spowoduje automatyczne wypełnienie pól tekstowych odpowiednimi wartościami.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![Modalne okno podręczne powoduje odświeżenie strony](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

Modalne okno podręczne powoduje odświeżenie strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-modalpopup-with-a-repeater-control-cs.md)
> [dalej](positioning-a-modalpopup-cs.md)
