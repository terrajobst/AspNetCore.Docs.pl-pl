---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Wyłączanie akcje podczas animacji (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Obsługuje ona również akcji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7862c5026a48fbee6eb48beb411e5e1d60c8b406
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="disabling-actions-during-animation-c"></a>Wyłączanie działania podczas animacji (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Obsługuje ona również akcje, takie jak kliknięcia myszą. Jednak po uruchomieniu animacji na kliknięcie myszą jest pożądane, aby wyłączyć kliknięć myszą podczas animacji.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Obsługuje ona również akcje, takie jak kliknięcia myszą. Jednak po uruchomieniu animacji na kliknięcie myszą jest pożądane, aby wyłączyć kliknięć myszą podczas animacji.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Animacji zostanie zastosowana do przycisku HTML następująco:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Należy pamiętać, że kontrolkę HTML jest używana zamiast formant sieci Web, ponieważ nie chcemy przycisk, aby utworzyć odświeżania strony; jest on tylko Uruchom animacji po stronie klienta dla instytucji.

Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

W ramach `<Animations>` węzła, `<OnClick>` jest elementem prawo do obsługi kliknięcia myszą. Jednak przycisk można kliknięty podczas animacji, jak również. `<EnableAction>` Zająć tego elementu. Ustawienie `Enabled="false"` wyłącza przycisk jako część animacji. Ponieważ używamy kilka poszczególnych animacji (wyłączenie przycisku i rzeczywistego animacje), `<Parallel>` element jest wymagany do przyklej pojedynczego animacji w jednej. W tym miejscu jest pełny kod znaczników dla `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Również będzie można ponownie włączyć do przycisku po animacji, przy użyciu następujących elementów XML na końcu listy:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Jednak w danym scenariuszu to bezużyteczne od przycisku znika i nie jest widoczny po zakończeniu animacji.


[![Przycisk jest wyłączona, jak działa animacji](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Przycisk jest wyłączona, jak działa animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](animating-in-response-to-user-interaction-cs.md)
> [dalej](triggering-an-animation-in-another-control-cs.md)
