---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Dostosowywanie indeksu z kontrolki DropShadow (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia. Jednak to w tle czasami powoduje konflikt z innych formantów, aby uzyskać Zainstaluj...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 5cee2acfac74c0790a7ad82bbfb503a8a16f6b47
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756432"
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Dostosowywanie indeksu z kontrolki DropShadow (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia. Jednak to w tle czasami jest w konflikcie z innymi formantami, na przykład formant Menu platformy ASP.NET. Gdy wpis menu pojawia się, prawdopodobnie za cienia.


## <a name="overview"></a>Omówienie

Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia. Jednak to w tle czasami jest w konflikcie z innymi formantami, na przykład formant Menu platformy ASP.NET. Gdy wpis menu pojawia się, prawdopodobnie za cienia.

## <a name="steps"></a>Kroki

Kod rozpocznie się za pomocą panelu, zawierające wystarczającej ilości tekstu, dzięki czemu na panelu zawiera wystarczająco dużo tekstu dla efektu były widoczne:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Kolejny panel jest umieszczane bezpośrednio przed `panelShadow` panelu. Zawiera menu w orientacji poziomej, tak, aby elementy menu są wyświetlane nad (lub zamiast: w obszarze) `dropShadow` panelu):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Następnie `DropShadowExtender` jest dodawany do rozszerzenia `panelShadow` panelu z efektem cienia:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Na koniec ASP.NET AJAX `ScriptManager` control umożliwia kontrolki zestawu narzędzi do pracy:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Po uruchomieniu tego skryptu, elementy menu wyświetlane w obszarze panelu. Jednak menu używa klasy CSS `panel` gdzie wystarczy zdefiniować dwie czynności w celu zapewnienia elementy są wyświetlane przed innymi panelu:

- Pozycjonowanie względne
- Dodatnia indeks

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Następnie `DropShadowExtender` formantu nie powoduje już konfliktu z formant Menu.


[![Przed: Wpis menu nie jest widoczny](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Przed: Wpis menu nie jest widoczny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![Po: Pojawi się wpis menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Po: Pojawi się wpis menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Next](manipulating-dropshadow-properties-from-client-code-cs.md)
