---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Zezwalanie tylko na niektóre znaki w polu tekstowym (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: e44b69a4f7d46f1f1278f7de07a2e6c025f5c316
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386763"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Zezwalanie tylko na niektóre znaki w polu tekstowym (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowe znaki i podjęcie próby można przesłać formularza.


## <a name="overview"></a>Omówienie

Kontrolek weryfikacji platformy ASP.NET można upewnić się, że tylko na niektóre znaki są dozwolone w danych wejściowych użytkownika. Jednak to nadal nie uniemożliwia użytkownikom wpisywanie nieprawidłowe znaki i podjęcie próby można przesłać formularza.

## <a name="steps"></a>Kroki

ASP.NET AJAX Control Toolkit zawiera `FilteredTextBox` kontrolki, która rozszerza pola tekstowego. Po uaktywnieniu tylko w określonym zestawie znaków mogą być wprowadzane w polu.

Aby to zrobić, najpierw należy w zwykły sposób ASP.NET AJAX `ScriptManager` która ładuje bibliotek JavaScript, które są również używane przez program ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

W efekcie potrzebujemy pola tekstowego:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Na koniec `FilteredTextBoxExtender` kontrolka zajmuje się ograniczenie znaków, użytkownik może wpisać. Najpierw ustaw `TargetControlID` atrybutu `ID` z `TextBox` kontroli. Następnie wybierz jedną z dostępnych `FilterType` wartości:

- `Custom` Domyślnie; Musisz podać listę prawidłowe znaki
- `LowercaseLetters` małe litery
- `Numbers` tylko cyfry
- `UppercaseLetters` tylko wielkie litery

Jeśli `Custom FilterType` jest używany, `ValidChars` właściwość musi być ustawione i podać listę znaków, które mogą być wpisana. Przy okazji: Jeśli użytkownik próbuje wkleić tekst w polu tekstowym, zostaną usunięte wszystkie nieprawidłowe znaki.

Oto znaczniki dla `FilteredTextBoxExtender` formant, który dopuszcza tylko cyfr (coś, który także byłby możliwego `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Uruchomić stronę i spróbuj wprowadzić literą, jeśli włączona JavaScript, nie będzie on działał; cyfry są jednak wyświetlane na stronie. Jednak należy pamiętać, że ochrona `FilteredTextBox` zapewnia nie jest dowód punktor: Jeśli JavaScript jest włączony, wszelkie dane mogą być wprowadzane w polu tekstowym, więc trzeba użyć oznacza, że dodatkowe sprawdzenie poprawności, czyli ASP. Formanty sprawdzania poprawności w sieci.


[![Mogą być wprowadzane tylko cyfry](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Mogą być wprowadzane tylko cyfry ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](allowing-only-certain-characters-in-a-text-box-cs.md)
