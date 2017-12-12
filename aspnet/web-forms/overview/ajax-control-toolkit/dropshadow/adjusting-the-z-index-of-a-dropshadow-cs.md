---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: "Dopasowywanie indeks cień (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Jednak ta tle czasami powoduje konflikt z inne formanty, zainstaluj..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 161f02daa5dd1f0e21853c1b7c1a65c1a9aa5d03
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Dopasowywanie indeks cień (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Jednak ta tle czasami powoduje konflikt z inne formanty, na przykład kontrolka Menu programu ASP.NET. Gdy pozycja menu pojawia się, prawdopodobnie za cień.


## <a name="overview"></a>Omówienie

Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Jednak ta tle czasami powoduje konflikt z inne formanty, na przykład kontrolka Menu programu ASP.NET. Gdy pozycja menu pojawia się, prawdopodobnie za cień.

## <a name="steps"></a>Kroki

Kod rozpocznie się z panelem, zawierające wystarczającej ilości tekstu, dzięki czemu panelu zawiera za mało tekst dla efektu, które mają być wyświetlane:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Inny panel znajduje się bezpośrednio przed `panelShadow` panelu. Zawiera menu orientacji poziomej tak, aby elementy menu są wyświetlane nad (lub zamiast: w obszarze) `dropShadow` panelu):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Następnie `DropShadowExtender` jest dodawany do rozszerzania `panelShadow` panel z efektem cienia:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Na koniec ASP.NET AJAX `ScriptManager` formant umożliwia kontroli zestawu narzędzi do pracy:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Po uruchomieniu tego skryptu, elementy menu są wyświetlane poniżej panelu. Jednak menu używa klasy CSS `panel` gdzie wystarczy zdefiniować dwie czynności, aby elementy pojawiają się przed innymi panelu:

- Pozycjonowanie względne
- Dodatnia indeks

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Następnie `DropShadowExtender` formantu nie powoduje konfliktu już z Menu kontroli.


[![Przed: Wpisu menu nie jest widoczny](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Przed: Nie jest widoczna pozycja menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![Po: Pojawi się wpis menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Po: Pojawi się wpis menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

>[!div class="step-by-step"]
[Dalej](manipulating-dropshadow-properties-from-client-code-cs.md)
