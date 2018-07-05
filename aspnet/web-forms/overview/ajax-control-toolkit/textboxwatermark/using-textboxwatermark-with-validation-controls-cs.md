---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Używanie kontrolki TextBoxWatermark z kontrolkami walidacji (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki TextBoxWatermark w zestawu narzędzi AJAX Control Toolkit rozszerza polu tekstowym, więc, że tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, go czy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: fe28f169e5e714ba07d8a71bb33d0f62608d688b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380694"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Używanie kontrolki TextBoxWatermark z kontrolkami walidacji (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Kontrolki TextBoxWatermark w zestawu narzędzi AJAX Control Toolkit rozszerza polu tekstowym, więc, że tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, będzie pusta. Jeśli użytkownik opuści pole bez konieczności wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. Może to kolidować z kontrolkami walidacji platformy ASP.NET na tej samej stronie, ale może być można rozwiązać te problemy.


## <a name="overview"></a>Omówienie

`TextBoxWatermark` Formantu w zestawu narzędzi AJAX Control Toolkit rozszerza pola tekstowego, więc, że tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, będzie pusta. Jeśli użytkownik opuści pole bez konieczności wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. Może to kolidować z kontrolkami walidacji platformy ASP.NET na tej samej stronie, ale może być można rozwiązać te problemy.

## <a name="steps"></a>Kroki

Konfiguracja podstawowa próbki jest następująca: `TextBox` kontroli jest znakiem wodnym przy użyciu `TextBoxWatermarkExtender` kontroli. Przycisk wyzwala odświeżenie strony i później będzie służyć do wyzwolenia formanty sprawdzania poprawności na stronie. Ponadto `ScriptManager` sterowania jest wymagany do zainicjowania ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Teraz Dodaj `RequiredFieldValidator` formant, który sprawdza, czy znajduje się tekst w polu po przesłaniu formularza. `InitialValue` Właściwości modułu sprawdzania poprawności, należy określić tę samą wartość, która jest używana w `TextBoxWatermarkExtender` kontroli: po przesłaniu formularza, wartości bez zmian pole tekstowe jest wartość limitu znajdujący się w nim:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Jednak jeden problem występuje w przypadku tej metody: Jeśli klient wyłącza JavaScript, pole tekstowe jest nie wstępnie przy użyciu tekstu znaku wodnego, w związku z tym `RequiredFieldValidator` nie spowoduje wyzwolenia komunikat o błędzie. W związku z tym, sekundy `RequiredFieldValidator` kontroli jest wymagana, która sprawdza, czy puste pole tekstowe (z pominięciem `InitialValue` atrybutu).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Ponieważ korzystać z obu modułów sprawdzania poprawności `Display` = `"Dynamic"`, użytkownik końcowy nie można odróżnić od wygląd, które z dwóch modułów sprawdzania poprawności zostało wywołane; zamiast tego należy prawdopodobnie wystąpił tylko jeden z nich.

Na koniec należy dodać kod po stronie serwera w danych wyjściowych tekst w polu, jeśli żadnego modułu weryfikacji wydany komunikat o błędzie:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![Moduł weryfikacji narzeka, że nie ma żadnego tekstu w polu](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Moduł weryfikacji narzeka, że nie ma żadnego tekstu w polu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-textboxwatermark-in-a-formview-cs.md)
> [dalej](using-textboxwatermark-in-a-formview-vb.md)
