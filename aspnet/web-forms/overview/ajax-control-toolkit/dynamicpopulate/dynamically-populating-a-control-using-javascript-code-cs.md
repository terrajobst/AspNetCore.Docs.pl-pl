---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: "Dynamicznie danymi formantu przy użyciu kodu JavaScript (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant DynamicPopulate w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wartość wynikową w formancie docelowym t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3b23e16f2e4dd26f50eb3e07f35d078dd19a19
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>Dynamicznie danymi formantu przy użyciu kodu JavaScript (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> DynamicPopulate formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołanie usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony. Istnieje również możliwość do wyzwolenia populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.


## <a name="overview"></a>Omówienie

`DynamicPopulate` Formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony. Istnieje również możliwość do wyzwolenia populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Przede wszystkim należy usługi sieci Web ASP.NET, która implementuje metodę do wywołania przez `DynamicPopulateExtender` formantu. Usługa sieci web implementuje metody `getDate()` spodziewa się jeden argument typu String, nazywany `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jednej kontekstu informacji z każdego wywołania usługi sieci web. Oto kod (plik `DynamicPopulate.cs.asmx`) która pobiera bieżącą datę w jednym z trzech formatów:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

W następnym kroku utwórz nową witrynę programu ASP.NET i uruchomić za pomocą formantu ASP.NET AJAX ScriptManager:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Następnie należy dodać formantu etykiety (na przykład za pomocą kontrolki HTML o takiej samej nazwie lub `<asp:Label />` formant sieci web) wykazujących później wynik wywołania usługi sieci web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Następnie należy uwzględnić `DynamicPopulateExtender` kontroli i podaj informacje o usługi sieci web, formantu docelowego, ale nie nazwę formantu, który wyzwala populacji będzie to później za pomocą niestandardowych skryptów JavaScript!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Teraz, aby części JavaScript. `$find()` Funkcja zdefiniowana przez biblioteki ASP.NET AJAX, zwraca odwołanie do obiektów po stronie serwera zestawu ASP.NET AJAX kontroli narzędzi, takich jak `DynamicPopulateExtender`. W bieżącym pliku `$find("dpe")` zwraca odwołanie do tego `DynamicPopulateExtender` kontrolki na stronie. Udostępnia ona metodę o nazwie `populate()` która wyzwala procesu dynamicznego populacji. `populate()` Metoda wymaga jednego argumentu: klucz kontekstu, która będzie służyć jako argument `getDate()` metody w sieci web. Tak na przykład `$find("dpe").populate("format1")` czy wypełnić etykiety z bieżącą datę w formacie dzień miesiąc rok.

Aby ustawić próbki bardziej elastyczne, użytkownik może teraz wybierać między kilka formatów daty. Dla każdego z nich jest wyświetlany przycisk radiowy. Po kliknięciu przycisku radiowego, kod JavaScript dynamicznie wypełnia etykiety z wybranego formatu daty. Poniżej przedstawiono tych przycisków radiowych:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Należy pamiętać, że w kontekście przycisku radiowego, wyrażenie JavaScript `this.value` odnosi się do wartości bieżącego przycisku do tych samych informacji `getDate()` metody może współpracować z.


[![Kliknięcie przycisku pobiera daty z serwera, w formacie określonym](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Kliknięcie przycisku pobiera daty z serwera, w formacie określonym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[Poprzednie](dynamically-populating-a-control-cs.md)
[dalej](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
