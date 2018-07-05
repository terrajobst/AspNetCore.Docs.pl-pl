---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Dynamiczne wypełnianie kontrolki przy użyciu kodu JavaScript (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wypełnia wynikowej wartości do formantu docelowego t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9df2a66bd49ecba52b0dd8b1d52a65b36c38a5dc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366851"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a>Dynamiczne wypełnianie kontrolki przy użyciu kodu JavaScript (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)

> Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony. Istnieje również możliwość wyzwalania populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.


## <a name="overview"></a>Omówienie

`DynamicPopulate` Kontrola w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony. Istnieje również możliwość wyzwalania populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta.

## <a name="steps"></a>Kroki

Po pierwsze, potrzebujesz usługi sieci Web ASP.NET, która implementuje metodę można wywoływać za pomocą `DynamicPopulateExtender` kontroli. Usługa sieci web implementuje metodę `getDate()` który oczekuje jednego argumentu typu ciąg, o nazwie `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jeden fragment informacje o kontekście przy każdym wywołaniem usługi sieci web. Oto kod (plik `DynamicPopulate.vb.asmx`) które pobiera bieżącą datę w jednym z trzech formatów:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

W następnym kroku utwórz nową witrynę programu ASP.NET i rozpoczynać formantu ScriptManager AJAX programu ASP.NET:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

Następnie dodaj kontrolkę typu etykieta (na przykład za pomocą kontrolki HTML o takiej samej nazwie lub `<asp:Label />` formant sieci web) będzie później przedstawiono w nich wynik wywołania usługi sieci web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

Następnie uwzględnić `DynamicPopulateExtender` kontroli i podaj informacje o usłudze sieci web, formantu docelowego, ale nie nazwę kontrolki, co powoduje wyzwolenie wypełniania, zostanie to wykonane w późniejszym czasie na przy użyciu niestandardowych skryptów JavaScript!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

Teraz, aby część języka JavaScript. `$find()` Funkcji, zdefiniowane przez bibliotekę ASP.NET AJAX zwraca odwołanie do obiektów po stronie serwera ASP.NET AJAX Control Toolkit, takich jak `DynamicPopulateExtender`. W bieżącym pliku `$find("dpe")` zwraca odwołanie do tego `DynamicPopulateExtender` kontrolki na stronie. Udostępnia metodę o nazwie `populate()` który wyzwala procesu dynamicznego populacji. `populate()` Metoda wymaga jednego argumentu: klucz kontekstu, która będzie służyć jako argument `getDate()` metodę sieci web. Dlatego na przykład `$find("dpe").populate("format1")` może wypełnić etykiety z bieżącą datę w formacie dzień miesiąc rok.

W celu próbki nieco bardziej elastyczne, użytkownik może teraz wybierać między kilka formatów daty. Dla każdego z nich jest wyświetlany przycisk radiowy. Po użytkownik kliknie przycisk radiowy kod JavaScript dynamicznie wypełnienie etykiety z wybranego formatu daty. Poniżej przedstawiono tych przycisków radiowych:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

Należy pamiętać, że w kontekście przycisk radiowy, wyrażenie JavaScript `this.value` odnosi się do wartości bieżącego przycisku, które akurat jest dokładnie te same informacje `getDate()` metoda może pracować.


[![Kliknięcie przycisku pobiera datę z serwera, w formacie określonym](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)

Kliknięcie przycisku pobiera datę z serwera, w formacie określonym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](dynamically-populating-a-control-vb.md)
> [dalej](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)
