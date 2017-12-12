---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: "Animacja w zależności od stanu (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Określa, czy animacja jest..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: cc8600f33f9c27e1045f5083a126b9d2d1e90303
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-vb"></a>Animacja w zależności od stanu (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Określa, czy animacja jest uruchomić lub nie można również są zależne od warunku w postaci kodu JavaScript.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Określa, czy animacja jest uruchomić lub nie można również są zależne od warunku w postaci kodu JavaScript.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe`runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchomienia animacji po całkowitym załadowaniem strony. Zamiast jedną z animacji regularne `<Condition>` element wejścia play. Kod JavaScript podany jako wartość `ConditionScript` atrybutu jest wykonywana w czasie wykonywania. Jeśli go daje w wyniku wartość true, animacji jest wykonywane, w przeciwnym razie nie. Następujący kod zawiera dwa animacji, każde z nich jest wykonywana w przypadkach na losowe 50%. Ponieważ może istnieć tylko jeden animacji w `<OnLoad>`, dwa `<Condition>` animacje są łączone ze sobą za pomocą `<Sequence>` elementu:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Należy pamiętać, że znak mniejszości (`<`) w `ConditionScript` atrybut musi być zmieniony (). Po Uruchom ten skrypt, albo nie uruchamia animacji jest jeden z dwóch lub czy obu.


[![Panel jest wygaszanie bez zmiany rozmiaru, więc drugi działa animacji, pierwsza z nich nie](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

Panel jest wygaszanie bez zmiany rozmiaru, więc drugi działa animacji, pierwsza z nich nie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animation-depending-on-a-condition-vb/_static/image3.png))

>[!div class="step-by-step"]
[Poprzednie](executing-several-animations-after-each-other-vb.md)
[dalej](picking-one-animation-out-of-a-list-vb.md)
