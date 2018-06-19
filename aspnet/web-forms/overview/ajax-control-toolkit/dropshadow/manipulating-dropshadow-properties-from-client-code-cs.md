---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulowanie właściwości cień z kodu klienta (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Dostosowywanie interfejsu edycji DataList
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870339"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>Manipulowanie właściwości cień z kodu klienta (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Właściwości tego rozszerzenia można także zmienić przy użyciu klienta kodu JavaScript.


## <a name="overview"></a>Omówienie

Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Właściwości tego rozszerzenia można także zmienić przy użyciu klienta kodu JavaScript.

## <a name="steps"></a>Kroki

Kod rozpoczyna się od panelu, zawierającą kilka wierszy tekstu:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

Skojarzona klasa CSS daje panelu Kolor tła nieuprzywilejowany:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

`DropShadowExtender` Jest dodawany do rozszerzania panel z efektem cienia, nieprzezroczystość ustawiona na 50%:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Następnie ASP.NET AJAX `ScriptManager` formant umożliwia kontroli zestawu narzędzi do pracy:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Inny panel zawiera dwa łącza JavaScript do ustawiania nieprzezroczystość cień: minus link zmniejsza nieprzezroczystość w tle, oraz link jej zwiększenie.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

Funkcja JavaScript `changeOpacity()` następnie musi najpierw odnaleźć `DropShadowExtender` formantu na stronie. Definiuje ASP.NET AJAX `$find()` metody dokładnie tego zadania. Następnie `get_Opacity()` metoda pobiera bieżący nieprzezroczystość `set_Opacity()` metody ustawia ją. Kod JavaScript następnie przełącza bieżącą wartość nieprzezroczystości `<label>` elementu:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![Nieprzezroczystość zostanie zmieniona po stronie klienta](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

Nieprzezroczystość zostanie zmieniona po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [dalej](adjusting-the-z-index-of-a-dropshadow-vb.md)
