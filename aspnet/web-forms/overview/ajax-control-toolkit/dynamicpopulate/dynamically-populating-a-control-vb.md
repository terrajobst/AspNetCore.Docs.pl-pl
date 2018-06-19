---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Dynamicznie danymi formantu (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant DynamicPopulate w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wartość wynikową w formancie docelowym t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e2031a80be71a406e632955583d83920dd0f3ef7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879741"
---
<a name="dynamically-populating-a-control-vb"></a>Dynamicznie danymi formantu (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> DynamicPopulate formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołanie usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony.


## <a name="overview"></a>Omówienie

`DynamicPopulate` Formantu w zestawie narzędzi programu ASP.NET AJAX kontroli wywołania usługi sieci web (lub metoda strony) i wypełnia wynikowej wartości do formantu docelowego na stronie bez odświeżania strony. W tym samouczku przedstawiono sposób tej konfiguracji.

## <a name="steps"></a>Kroki

Przede wszystkim należy usługi sieci Web ASP.NET, która implementuje metodę do wywołania przez `DynamicPopulate`. Klasa usługi sieci web wymaga `ScriptService` atrybut, który jest zdefiniowany w ramach `Microsoft.Web.Script.Services`; w przeciwnym razie ASP.NET AJAX nie można utworzyć serwera proxy JavaScript po stronie klienta dla usługi sieci web, która z kolei jest wymagana przez `DynamicPopulate`.

Metoda sieci web należy spodziewać się jeden argument typu String, o nazwie `contextKey`, ponieważ `DynamicPopulate` kontroli wysyła jednej kontekstu informacji z każdego wywołania usługi sieci web. Następująca usługa sieci web zwraca bieżącą datę w formacie reprezentowany przez `contextKey` argumentu:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Usługi sieci web jest następnie zapisywana jako `DynamicPopulate.vb.asmx`. Alternatywnie można zaimplementować `getDate()` metody jako metodę strony w rzeczywistych strony ASP.NET z `DynamicPopulate` formantu.

W następnym kroku utwórz nowy plik programu ASP.NET. Zawsze, pierwszym krokiem jest uwzględnienie `ScriptManager` na bieżącej stronie można załadować biblioteki ASP.NET AJAX i dla zapewnienia działania kontroli zestawu narzędzi:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Następnie należy dodać formantu etykiety (na przykład za pomocą kontrolki HTML o takiej samej nazwie lub &lt; `asp:Label`  / &gt; formant sieci web) wykazujących później wynik wywołania usługi sieci web.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Przycisk HTML (jako kontrolkę HTML, ponieważ nie wymaga ogłaszania zwrotnego na serwerze) zostanie następnie użyte do wyzwolenia dynamiczne wypełniania:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Na koniec należy `DynamicPopulateExtender` formantu do elementów danych przesyłanych w sieci w górę. Następujące atrybuty zostanie skonfigurowany (oprócz oczywiste te `ID` i `runat` = `"server"`):

- `TargetControlID` gdzie umieścić wyniki z wywołaniem usługi sieci web
- `ServicePath` Ścieżka do usługi sieci web (pominąć, jeśli chcesz użyć metody strony)
- `ServiceMethod` Nazwa metody sieci web lub strona — Metoda
- `ContextKey` informacje o kontekście do wysłania do usługi sieci web
- `PopulateTriggerControlID` element, który wyzwala wywołania usługi sieci web
- `ClearContentsDuringUpdate` Czy pusty element docelowy podczas wywołania usługi sieci web

Jak widać, formantu wymaga pewne informacje, ale przygotowania wszystko, co jest dość proste. Oto kod znaczników dla `DynamicPopulateExtender` formantu w tym scenariuszu:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Uruchom strony ASP.NET w przeglądarce, a następnie kliknij przycisk; otrzymasz bieżącą datę w formacie dzień miesiąc rok.


[![Kliknięcie przycisku pobiera daty z serwera](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Kliknięcie przycisku pobiera daty z serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [dalej](dynamically-populating-a-control-using-javascript-code-vb.md)
