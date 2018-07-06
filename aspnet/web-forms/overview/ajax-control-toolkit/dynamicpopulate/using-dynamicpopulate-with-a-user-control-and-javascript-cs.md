---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Używanie kontrolki DynamicPopulate z kontrolką użytkownika i kodu JavaScript (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wypełnia wynikowej wartości do formantu docelowego t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 3a380f24a521153cba084de21014380df49efabd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835175"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>Używanie kontrolki DynamicPopulate z kontrolką użytkownika i kodu JavaScript (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony. Istnieje również możliwość wyzwalania populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta. Jednak szczególną uwagę należy podjąć, gdy urządzenie extender znajduje się w kontrolce użytkownika.


## <a name="overview"></a>Omówienie

`DynamicPopulate` Kontrola w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony. Istnieje również możliwość wyzwalania populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta. Jednak szczególną uwagę należy podjąć, gdy urządzenie extender znajduje się w kontrolce użytkownika.

## <a name="steps"></a>Kroki

Po pierwsze, potrzebujesz usługi sieci Web ASP.NET, która implementuje metodę można wywoływać za pomocą `DynamicPopulateExtender` kontroli. Usługa sieci web implementuje metodę `getDate()` który oczekuje jednego argumentu typu ciąg, o nazwie `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jeden fragment informacje o kontekście przy każdym wywołaniem usługi sieci web. Oto kod (plik `DynamicPopulate.cs.asmx`) które pobiera bieżącą datę w jednym z trzech formatów:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

W następnym kroku, Utwórz nową kontrolkę użytkownika (`.ascx` pliku), są oznaczane przez następującą deklarację w jej pierwszy wiersz:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

A &lt; `label` &gt; element będzie używany do wyświetlania danych z serwera.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Również w pliku kontrolki użytkownika, firma Microsoft użyje trzy przyciski radiowe, każdy z nich reprezentuje jeden z trzech formatów możliwych Data obsługiwana przez usługę sieci web. Gdy użytkownik kliknie na jeden z przycisków radiowych, przeglądarka będzie wykonywać kod JavaScript, która wygląda następująco:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Ten kod uzyskuje dostęp do `DynamicPopulateExtender` (nie martw się o identyfikatorze dziwne, ale, zostanie to opisane w dalszej) i wyzwala dynamiczne wypełnianie danymi. W kontekście bieżącego przycisk radiowy `this.value` odwołuje się do jego wartość, która jest `format1`, `format2` lub `format3` dokładnie metody sieci web oczekiwaniom.

Jedyną czynnością, brakuje w kontrolce użytkownika jeszcze jest `DynamicPopulateExtender` kontrolowanie linki przycisków radiowych do usługi sieci web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Ponownie należy zaznaczyć otrzymano nieoczekiwany identyfikator używany w kontrolce: `mcd1$myDate` zamiast `myDate`. Wcześniej kod JavaScript używany `mcd1_dpe1` dostęp do `DynamicPopulateExtender` zamiast `dpe1`. Ta strategia nazewnictwa jest specjalnych wymagań, korzystając z `DynamicPopulateExtender` znajdujących się pod kontrolą użytkownika. Ponadto należy osadzić kontroli użytkownika w określony sposób, aby umożliwić jej pracę. Tworzenie nowej strony programu ASP.NET i zarejestrować prefiksu tagu kontrolki użytkownika, które zostało zaimplementowane:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Następnie wprowadzić ASP.NET AJAX `ScriptManager` kontroli na nowej stronie:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Na koniec Dodaj formant użytkownika do strony. Musisz ustawić jego `ID` atrybutu (i `runat="server"`, oczywiście), ale należy również ustawić ją na określonej nazwy: `mcd1` to prefiks używany w kontrolce użytkownika dostępu do niego przy użyciu języka JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

I to wszystko! Strona zachowuje się zgodnie z oczekiwaniami: użytkownik kliknie na jeden z przycisków radiowych, formant w zestawie narzędzi wywołuje usługę sieci web i wyświetla bieżącą datę w wybranym formacie.


[![Przyciski radiowe znajdują się w kontrolce użytkownika](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Przyciski radiowe znajdują się w kontrolce użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](dynamically-populating-a-control-using-javascript-code-cs.md)
> [dalej](dynamically-populating-a-control-vb.md)
