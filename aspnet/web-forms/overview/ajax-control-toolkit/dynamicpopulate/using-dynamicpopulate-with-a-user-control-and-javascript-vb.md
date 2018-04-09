---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Przy użyciu DynamicPopulate z formantu użytkownika i kodu JavaScript (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant DynamicPopulate w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wartość wynikową w formancie docelowym t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 715973ef4923e635ec2a860d00d55f13a5f8b2d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Przy użyciu DynamicPopulate z formantu użytkownika i kodu JavaScript (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> DynamicPopulate formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołanie usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony. Istnieje również możliwość do wyzwolenia populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta. Jednak szczególną uwagę brane rozszerzeń znajduje się w formancie użytkownika.


## <a name="overview"></a>Omówienie

`DynamicPopulate` Formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony. Istnieje również możliwość do wyzwolenia populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta. Jednak szczególną uwagę brane rozszerzeń znajduje się w formancie użytkownika.

## <a name="steps"></a>Kroki

Przede wszystkim należy usługi sieci Web ASP.NET, która implementuje metodę do wywołania przez `DynamicPopulateExtender` formantu. Usługa sieci web implementuje metody `getDate()` spodziewa się jeden argument typu String, nazywany `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jednej kontekstu informacji z każdego wywołania usługi sieci web. Oto kod (pliki `DynamicPopulate.vb.asmx`) która pobiera bieżącą datę w jednym z trzech formatów:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

W następnym kroku należy utworzyć nowy formant użytkownika (`.ascx` pliku), są oznaczane przez następujące oświadczenie w jego pierwszego wiersza:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

A &lt; `label` &gt; element będzie używany do wyświetlania danych z serwera.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

Również w pliku sterowania użytkownika, użyjemy trzy przyciski radiowe, każdą z nich reprezentujące jedną z trzech formatów daty możliwe obsługiwane przez usługę sieci web. Gdy użytkownik kliknie jeden z przycisków radiowych, przeglądarka będzie wykonywał kodu JavaScript, która wygląda następująco:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Ten kod uzyskuje dostęp do `DynamicPopulateExtender` (nie martw się o identyfikatorze dziwne jeszcze, to będzie uwzględnione w przyszłości) i wyzwala dynamiczne populacji z danymi. W kontekście bieżącego przycisku radiowego `this.value` odwołuje się do jego wartość, która jest `format1`, `format2` lub `format3` dokładnie co metoda sieci web oczekuje.

Jedyną operacją, brak jeszcze w formancie użytkownika jest `DynamicPopulateExtender` kontroli, która łączy przyciski radiowe z usługą sieci web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Ponownie należy zaznaczyć dziwne identyfikator używany w formancie: `mcd1$myDate` zamiast `myDate`. Wcześniej, kod JavaScript używane `mcd1_dpe1` dostępu `DynamicPopulateExtender` zamiast `dpe1`. Ta strategia nazewnictwa jest szczególne wymagania w przypadku używania `DynamicPopulateExtender` w formancie użytkownika. Ponadto należy osadzić kontroli użytkownika w określony sposób, aby było to działa. Tworzenie nowej strony ASP.NET i zarejestrować prefiksu tagu kontrolki użytkownika, które zostały zaimplementowane:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

Następnie wprowadzić ASP.NET AJAX `ScriptManager` kontrolki w nowej strony:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Na koniec Dodaj kontrolkę użytkownika do strony. Należy ustawić jego `ID` atrybutu (i `runat="server"`, oczywiście), ale również ma ustawioną nazwę: `mcd1` ponieważ jest to prefiks używany w formancie użytkownika dostępu do niej przy użyciu języka JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

I to już wszystko! Strona działa zgodnie z oczekiwaniami: użytkownik kliknie jeden z przycisków radiowych, formantu w zestawie narzędzi programu wywołania usługi sieci web i wyświetla bieżącą datę w wybranym formacie.


[![Przyciski radiowe znajdują się w formancie użytkownika](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

Przyciski radiowe znajdują się w formancie użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](dynamically-populating-a-control-using-javascript-code-vb.md)
