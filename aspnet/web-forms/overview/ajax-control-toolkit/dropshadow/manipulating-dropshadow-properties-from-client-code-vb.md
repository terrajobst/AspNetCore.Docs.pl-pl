---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: "Manipulowanie właściwości cień z kodu klienta (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Właściwości tego rozszerzenia można zmienić za pomocą klienta JavaScrip..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 706ccb5a95873aad4c1b9e0942ab06cf4162710a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Manipulowanie właściwości cień z kodu klienta (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Właściwości tego rozszerzenia można także zmienić przy użyciu klienta kodu JavaScript.


## <a name="overview"></a>Omówienie

Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Właściwości tego rozszerzenia można także zmienić przy użyciu klienta kodu JavaScript.

## <a name="steps"></a>Kroki

Kod rozpoczyna się od panelu, zawierającą kilka wierszy tekstu:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

Skojarzona klasa CSS daje panelu Kolor tła nieuprzywilejowany:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

`DropShadowExtender` Jest dodawany do rozszerzania panel z efektem cienia, nieprzezroczystość ustawiona na 50%:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Następnie ASP.NET AJAX `ScriptManager` formant umożliwia kontroli zestawu narzędzi do pracy:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Inny panel zawiera dwa łącza JavaScript do ustawiania nieprzezroczystość cień: minus link zmniejsza nieprzezroczystość w tle, oraz link jej zwiększenie.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

Funkcja JavaScript `changeOpacity()` następnie musi najpierw odnaleźć `DropShadowExtender` formantu na stronie. Definiuje ASP.NET AJAX `$find()` metody dokładnie tego zadania. Następnie `get_Opacity()` metoda pobiera bieżący nieprzezroczystość `set_Opacity()` metody ustawia ją. Kod JavaScript następnie przełącza bieżącą wartość nieprzezroczystości `<label>` elementu:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![Nieprzezroczystość zostanie zmieniona po stronie klienta](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

Nieprzezroczystość zostanie zmieniona po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[Poprzednie](adjusting-the-z-index-of-a-dropshadow-vb.md)
