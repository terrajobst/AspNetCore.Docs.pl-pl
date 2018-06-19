---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Wykonywanie kilku animacji po sobie nawzajem (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Umożliwia uruchamianie severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 700946b9f32c5ed2dcb8586e7c0e84d2238ff103
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873761"
---
<a name="executing-several-animations-after-each-other-vb"></a>Wykonywanie kilku animacji po sobie nawzajem (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Umożliwia uruchamianie kilka animacji jeden po drugim.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Umożliwia uruchamianie kilka animacji jeden po drugim.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchomienia animacji po całkowitym załadowaniem strony. Ogólnie rzecz biorąc `<OnLoad>` akceptuje tylko jednej animacji. Framework animacji można sprzęgać kilka animacji w jednej przy użyciu `<Sequence>` elementu. Wszystkie animacje w `<Sequence>` są wykonywane jeden po drugim. Oto możliwe kod znaczników dla `AnimationExtender` kontroli, najpierw udostępniania panelu szersze i następnie zmniejszenie jego wysokość:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

Po uruchomieniu tego skryptu panelu otrzymuje szersze, a następnie mniejszy.


[![Najpierw zwiększa się szerokość](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

Najpierw zwiększa się szerokość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image3.png))


[![Następnie zostaje zmniejszona wysokość](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

A następnie zostaje zmniejszona wysokość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-several-animations-at-the-same-time-vb.md)
> [dalej](animation-depending-on-a-condition-vb.md)
