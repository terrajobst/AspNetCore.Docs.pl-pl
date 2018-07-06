---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Wyłączanie akcji podczas animacji (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Obsługuje ona również działanie...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 396411332735503d1781e171bead8ab6aaed6efa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828198"
---
<a name="disabling-actions-during-animation-c"></a>Wyłączanie akcji podczas animacji (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Obsługuje ona również akcje, takie jak kliknięcia myszą. Jednak po uruchomieniu przez kliknięcie myszą animacji jest pożądane, aby wyłączyć kliknięć myszą podczas animacji.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Obsługuje ona również akcje, takie jak kliknięcia myszą. Jednak po uruchomieniu przez kliknięcie myszą animacji jest pożądane, aby wyłączyć kliknięć myszą podczas animacji.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Animacja zostaną zastosowane do przycisku HTML następująco:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Należy pamiętać, że formant HTML jest używana zamiast formantu sieci Web, ponieważ nie chcemy przycisk, aby utworzyć zwrotu; po prostu są to uruchomienie animacji po stronie klienta dla nas.

Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

W ramach `<Animations>` węzła `<OnClick>` jest odpowiednich elementów do obsługi kliknięcia myszą. Jednak przycisku można kliknął podczas animacji, jak również. `<EnableAction>` Elementu może zajmować się tego. Ustawienie `Enabled="false"` wyłącza przycisk jako część animacji. Ponieważ używamy kilku poszczególnych animacji (wyłączenie przycisku i rzeczywiste animacji), `<Parallel>` element jest wymagany do przyklej jednej animacji ze sobą w jednym. Oto kompletny kod znaczników dla `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Byłoby to możliwe ponownie włączyć przycisk po animacji, przy użyciu następujących elementów XML na końcu listy:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Jednak w danym scenariuszu byłoby to bezcelowe od przycisku znika i nie jest widoczny na końcu animacji.


[![Przycisk został wyłączony, tak szybko, jak działa animacji](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Przycisk został wyłączony, tak szybko, jak działa animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](animating-in-response-to-user-interaction-cs.md)
> [dalej](triggering-an-animation-in-another-control-cs.md)
