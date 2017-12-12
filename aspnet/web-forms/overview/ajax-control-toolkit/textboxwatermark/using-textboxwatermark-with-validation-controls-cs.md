---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: "Przy użyciu TextBoxWatermark z formantami sprawdzania poprawności (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant TextBoxWatermark w zestawie narzędzi kontroli AJAX rozszerza pole tekstowe tak, aby tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu go i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 61fa55c8c4580800de1097b7242c7077cda27115
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Przy użyciu TextBoxWatermark z formantami sprawdzania poprawności (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Formant TextBoxWatermark w zestawie narzędzi kontroli AJAX rozszerza pole tekstowe tak, aby tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, jest opróżniany. Jeśli użytkownik opuści pole bez wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. To może dojść do kolizji z formantami weryfikacji platformy ASP.NET na tej samej stronie, ale może można rozwiązać te problemy.


## <a name="overview"></a>Omówienie

`TextBoxWatermark` Formantu w zestawie narzędzi kontroli AJAX rozszerza pola tekstowego, aby tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, jest opróżniany. Jeśli użytkownik opuści pole bez wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. To może dojść do kolizji z formantami weryfikacji platformy ASP.NET na tej samej stronie, ale może można rozwiązać te problemy.

## <a name="steps"></a>Kroki

Podstawowa konfiguracja próbki jest następująca: `TextBox` kontroli jest oznaczone znakiem wodnym przy użyciu `TextBoxWatermarkExtender` formantu. Przycisk wyzwala odświeżania strony i później będzie służyć do wyzwolenia formanty walidacji na tej stronie. Ponadto `ScriptManager` formant jest wymagany do zainicjowania ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Teraz Dodaj `RequiredFieldValidator` formant, który sprawdza, czy jest tekst w polu po przesłaniu formularza. `InitialValue` Właściwości modułu sprawdzania poprawności musi mieć ustawioną taką samą wartość, która jest używana w `TextBoxWatermarkExtender` kontroli: po przesłaniu formularza, wartości bez zmian pole tekstowe jest wartość znaku wodnego w niej:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Jednak jest jednym z problemów z tej metody: Jeśli klient wyłącza JavaScript, pola tekstowego jest nie wstępnie z tekstu znaku wodnego, w związku z tym `RequiredFieldValidator` nie spowoduje wyzwolenia komunikat o błędzie. W związku z tym drugiej `RequiredFieldValidator` kontroli jest wymagana, która sprawdza, czy pole puste (pominięcie `InitialValue` atrybutu).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Ponieważ korzystać z obu modułów sprawdzania poprawności `Display` = `"Dynamic"`, użytkownik końcowy nie może odróżnić od wyglądu, którego dwa moduły weryfikacji zostało zainicjowane; należy prawdopodobnie wystąpił tylko jeden z nich.

Na koniec należy dodać kodu po stronie serwera do wyjściowego tekst w polu, jeśli żadnego modułu weryfikacji wystawiony komunikat o błędzie:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![Moduł weryfikacji narzeka, że nie ma żadnego tekstu w polu](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Moduł weryfikacji narzeka, że nie ma żadnego tekstu w polu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

>[!div class="step-by-step"]
[Poprzednie](using-textboxwatermark-in-a-formview-cs.md)
[dalej](using-textboxwatermark-in-a-formview-vb.md)
