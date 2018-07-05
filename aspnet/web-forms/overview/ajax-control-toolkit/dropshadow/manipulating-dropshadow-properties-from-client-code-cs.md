---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulowanie właściwościami DropShadow z poziomu kodu klienta (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Dostosowywanie interfejsu edycji kontrolki DataList
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e3166b9da97a0f4097566b62ba52b6d672eab78f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377288"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>Manipulowanie właściwościami DropShadow z poziomu kodu klienta (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia. Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.


## <a name="overview"></a>Omówienie

Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia. Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.

## <a name="steps"></a>Kroki

Panel, zawierający kilka wierszy tekstu zaczyna się kod:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

Skojarzona klasa CSS zapewnia panelu Kolor tła nieuprzywilejowany:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

`DropShadowExtender` Jest dodawany do rozszerzenia panelu z efektem cienia, nieprzezroczystość równa 50%:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Następnie ASP.NET AJAX `ScriptManager` control umożliwia kontrolki zestawu narzędzi do pracy:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Kolejny panel zawiera dwa linki JavaScript do ustawiania Nieprzezroczystość cienia: minus link zmniejsza jego nieprzezroczystości, oraz link jej zwiększenie.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

Funkcja języka JavaScript `changeOpacity()` następnie musi najpierw odnaleźć `DropShadowExtender` formantu na stronie. Definiuje ASP.NET AJAX `$find()` metody do dokładnie tego zadania. Następnie `get_Opacity()` metoda pobiera bieżący nieprzezroczystość `set_Opacity()` metody ustawia ją. Następnie kod JavaScript umieszcza bieżącą wartość nieprzezroczystości w `<label>` elementu:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![Nieprzezroczystość zostanie zmieniony po stronie klienta](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

Nieprzezroczystość zostanie zmieniony po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [dalej](adjusting-the-z-index-of-a-dropshadow-vb.md)
