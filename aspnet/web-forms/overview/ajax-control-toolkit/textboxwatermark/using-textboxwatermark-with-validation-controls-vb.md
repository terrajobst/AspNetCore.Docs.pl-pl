---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Używanie kontrolki TextBoxWatermark z kontrolkami walidacji (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki TextBoxWatermark w zestawu narzędzi AJAX Control Toolkit rozszerza polu tekstowym, więc, że tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, go czy...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 636a9d00b4f699536d2851d3bac5f657c272c80a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753934"
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a>Używanie kontrolki TextBoxWatermark z kontrolkami walidacji (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)

> Kontrolki TextBoxWatermark w zestawu narzędzi AJAX Control Toolkit rozszerza polu tekstowym, więc, że tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, będzie pusta. Jeśli użytkownik opuści pole bez konieczności wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. Może to kolidować z kontrolkami walidacji platformy ASP.NET na tej samej stronie, ale może być można rozwiązać te problemy.


## <a name="overview"></a>Omówienie

`TextBoxWatermark` Formantu w zestawu narzędzi AJAX Control Toolkit rozszerza pola tekstowego, więc, że tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, będzie pusta. Jeśli użytkownik opuści pole bez konieczności wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. Może to kolidować z kontrolkami walidacji platformy ASP.NET na tej samej stronie, ale może być można rozwiązać te problemy.

## <a name="steps"></a>Kroki

Konfiguracja podstawowa próbki jest następująca: `TextBox` kontroli jest znakiem wodnym przy użyciu `TextBoxWatermarkExtender` kontroli. Przycisk wyzwala odświeżenie strony i później będzie służyć do wyzwolenia formanty sprawdzania poprawności na stronie. Ponadto `ScriptManager` sterowania jest wymagany do zainicjowania ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

Teraz Dodaj `RequiredFieldValidator` formant, który sprawdza, czy znajduje się tekst w polu po przesłaniu formularza. `InitialValue` Właściwości modułu sprawdzania poprawności, należy określić tę samą wartość, która jest używana w `TextBoxWatermarkExtender` kontroli: po przesłaniu formularza, wartości bez zmian pole tekstowe jest wartość limitu znajdujący się w nim:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

Jednak jeden problem występuje w przypadku tej metody: Jeśli klient wyłącza JavaScript, pole tekstowe jest nie wstępnie przy użyciu tekstu znaku wodnego, w związku z tym `RequiredFieldValidator` nie spowoduje wyzwolenia komunikat o błędzie. W związku z tym, sekundy `RequiredFieldValidator` kontroli jest wymagana, która sprawdza, czy puste pole tekstowe (z pominięciem `InitialValue` atrybutu).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

Ponieważ korzystać z obu modułów sprawdzania poprawności `Display` = `"Dynamic"`, użytkownik końcowy nie można odróżnić od wygląd, które z dwóch modułów sprawdzania poprawności zostało wywołane; zamiast tego należy prawdopodobnie wystąpił tylko jeden z nich.

Na koniec należy dodać kod po stronie serwera w danych wyjściowych tekst w polu, jeśli żadnego modułu weryfikacji wydany komunikat o błędzie:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


[![Moduł weryfikacji narzeka, że nie ma żadnego tekstu w polu](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)

Moduł weryfikacji narzeka, że nie ma żadnego tekstu w polu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-textboxwatermark-in-a-formview-vb.md)
