---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Dynamiczne wypełnianie kontrolki (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wypełnia wynikowej wartości do formantu docelowego t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: fd55b59f9375eb320711ffea8d971a8d86179c43
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368663"
---
<a name="dynamically-populating-a-control-vb"></a>Dynamiczne wypełnianie kontrolki (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> Kontrolki DynamicPopulate w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony.


## <a name="overview"></a>Omówienie

`DynamicPopulate` Kontrola w ASP.NET AJAX Control Toolkit wywołuje usługę sieci web (lub metody korzystającej ze strony) i wprowadza wartość wynikową do formantu docelowego na stronie bez odświeżania strony. W tym samouczku pokazano, jak to skonfigurować.

## <a name="steps"></a>Kroki

Po pierwsze, potrzebujesz usługi sieci Web ASP.NET, która implementuje metodę można wywoływać za pomocą `DynamicPopulate`. Klasa usługi sieci web wymaga `ScriptService` atrybutu, która jest zdefiniowana w ramach `Microsoft.Web.Script.Services`; w przeciwnym razie ASP.NET AJAX, nie można utworzyć serwera proxy JavaScript po stronie klienta dla usługi sieci web, która z kolei jest wymagana przez `DynamicPopulate`.

Metoda sieci web należy oczekiwać jeden argument typu String, o nazwie `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jeden fragment informacje o kontekście przy każdym wywołaniem usługi sieci web. Następująca usługa sieci web zwraca bieżącą datę w formacie, reprezentowane przez `contextKey` argumentu:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Usługa sieci web, następnie jest zapisywany jako `DynamicPopulate.vb.asmx`. Alternatywnie można zaimplementować `getDate()` metodę jako metodę stron w ciągu rzeczywistą stronę ASP.NET za pomocą `DynamicPopulate` kontroli.

W następnym kroku utwórz nowy plik platformy ASP.NET. Jak zawsze, pierwszym krokiem jest uwzględnienie `ScriptManager` na bieżącej stronie, można załadować biblioteki ASP.NET AJAX i pracy zestaw narzędzi do sterowania:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Następnie dodaj kontrolkę typu etykieta (na przykład za pomocą kontrolki HTML o takiej samej nazwie lub &lt; `asp:Label`  / &gt; formant sieci web) będzie później przedstawiono w nich wynik wywołania usługi sieci web.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Przycisk HTML (jako kontrolkę HTML, ponieważ nie wymaga zwrot do serwera), następnie będzie służyć do wyzwolenia dynamiczne wypełnianie:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Na koniec musimy `DynamicPopulateExtender` kontroli coś o komunikacji sieciowej w górę. Następujące atrybuty zostaną ustawione (oprócz widocznych te `ID` i `runat` = `"server"`):

- `TargetControlID` gdzie umieścić wyniki z wywołaniem usługi sieci web
- `ServicePath` Ścieżka do usługi sieci web (pominąć, jeśli chcesz użyć metody strony)
- `ServiceMethod` Nazwa metody sieci web lub metody korzystającej ze strony
- `ContextKey` informacje o kontekście do wysłania do usługi sieci web
- `PopulateTriggerControlID` element, co powoduje wyzwolenie wywołania usługi sieci web
- `ClearContentsDuringUpdate` Czy chcesz opróżnić elementu docelowego podczas wywołania usługi sieci web

Jak widać, którego wymaga formant pewne informacje, ale umieszczenia wszystkiego w miejscu jest bardzo proste. Oto znaczniki dla `DynamicPopulateExtender` kontrolki, w tym scenariuszu:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Uruchamianie jej w przeglądarce strony ASP.NET, a następnie kliknij przycisk; otrzymasz bieżącą datę w formacie dzień miesiąc rok.


[![Kliknięcie przycisku pobiera datę z serwera](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Kliknięcie przycisku pobiera datę z serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [dalej](dynamically-populating-a-control-using-javascript-code-vb.md)
