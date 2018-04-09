---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Pobrania jednej animacji spoza listy (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Platformę również zez...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 2d4aac447fcdfbf296560091cfcdf5eb51997a7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="picking-one-animation-out-of-a-list-c"></a>Pobrania jednej animacji spoza listy (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Platformę umożliwia także programisty i wybierz jedną animację poza listę animacji, w zależności od wersji ewaluacyjnej kod JavaScript.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Platformę umożliwia także programisty i wybierz jedną animację poza listę animacji, w zależności od wersji ewaluacyjnej kod JavaScript.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchomienia animacji po całkowitym załadowaniem strony. Zamiast jedną z animacji regularne `<Case>` element wejścia play. Jest oceniana wartość jego atrybutu SelectScript; zwracana wartość musi być numeryczny. W zależności od tego numeru, jeden subanimations w &lt;przypadku&gt; jest wykonywana. Na przykład, jeśli SelectScript 2, Toolkit kontroli uruchamia trzeci animacji w &lt;przypadku&gt; (zliczanie rozpoczyna się od 0).

Następujący kod definiuje trzy subanimations: zmiana rozmiaru szerokość, wysokość rozmiaru i wygaszanie. Kod JavaScript (`Math.floor(3 * Math.random())`) następnie wybiera liczbą z zakresu od 0 do 2, tak aby jedną z trzech animacji jest uruchamiane:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


[![Jedną z możliwych animacji trzy: poszerzania panelu](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

Jedną z możliwych animacji trzy: poszerzania panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](picking-one-animation-out-of-a-list-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](animation-depending-on-a-condition-cs.md)
> [dalej](animating-in-response-to-user-interaction-cs.md)
