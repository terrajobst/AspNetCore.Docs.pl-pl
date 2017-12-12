---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: "Zezwalanie tylko niektórych znaków w polu tekstowym (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formanty weryfikacji platformy ASP.NET można upewnij się, czy tylko niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal uniemożliwia użytkownikom wpisywania nieprawidłowy..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: 246c3b5dd55ceb0f47ad1f4982ae5b3bf855e747
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a>Zezwalanie tylko niektórych znaków w polu tekstowym (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)

> Formanty weryfikacji platformy ASP.NET można upewnij się, czy tylko niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal nie uniemożliwić użytkownikom wpisując nieprawidłowych znaków, a w trakcie przesyłania formularza.


## <a name="overview"></a>Omówienie

Formanty weryfikacji platformy ASP.NET można upewnij się, czy tylko niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal nie uniemożliwić użytkownikom wpisując nieprawidłowych znaków, a w trakcie przesyłania formularza.

## <a name="steps"></a>Kroki

Zawiera zestawie narzędzi programu ASP.NET AJAX kontroli `FilteredTextBox` kontrolują, które rozszerza pola tekstowego. Po uaktywnieniu w polu można wprowadzać tylko w określonym zestawie znaków.

Aby to zrobić, najpierw należy w zwykły sposób ASP.NET AJAX `ScriptManager` który ładuje bibliotekach JavaScript, które są również używane w zestawie narzędzi programu ASP.NET AJAX sterowania:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

Następnie potrzebujemy pole tekstowe:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

Na koniec `FilteredTextBoxExtender` kontroli zajmuje się ograniczenie znaków, użytkownik może wpisać. Najpierw należy ustawić `TargetControlID` atrybutu `ID` z `TextBox` formantu. Następnie wybierz jedną z dostępnych `FilterType` wartości:

- `Custom`Domyślnie; należy podać listę prawidłowe znaki
- `LowercaseLetters`małe litery
- `Numbers`tylko cyfr
- `UppercaseLetters`tylko wielkie litery

Jeśli `Custom FilterType` jest używana, `ValidChars` właściwości musi być ustawione i zawierają listę znaków, które mogą zostać wpisany. Sposobem: próba Wklej tekst w polu tekstowym zostaną usunięte wszystkie nieprawidłowe znaki.

Oto kod znaczników dla `FilteredTextBoxExtender` formant, który zezwala tylko cyfry (coś, co także byłoby możliwe za pomocą `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

Uruchomić stronę i spróbuj wprowadzić literę, czy włączyć obsługę języka JavaScript, nie będzie on działał; cyfr jednak wyświetlane na stronie. Jednak należy pamiętać, że ochrony `FilteredTextBox` zapewnia nie jest potwierdzenie punktor: Jeśli JavaScript jest włączony, wszelkie dane mogą być wprowadzane w polu tekstowym, należy użyć oznacza, że dodatkowe sprawdzenie poprawności, tj. ASP. Formanty walidacji w sieci.


[![Można wprowadzać tylko cyfry](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

Można wprowadzać tylko cyfry ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))

>[!div class="step-by-step"]
[Dalej](allowing-only-certain-characters-in-a-text-box-vb.md)
