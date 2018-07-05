---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animacja w zależności od warunku (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Czy animacja jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: c28f4583e6f0d1bb5c1438322980a44aa53fbd89
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391366"
---
<a name="animation-depending-on-a-condition-c"></a>Animacja w zależności od warunku (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Czy animacji jest uruchomiony lub nie może także zależeć od warunku w postaci kodu JavaScript.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Czy animacji jest uruchomiony lub nie może także zależeć od warunku w postaci kodu JavaScript.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchamiania animacji, po całkowitym załadowaniu strony. Zamiast jednej z regularnych animacji `<Condition>` element, który jest dostarczany do gry. Kod JavaScript, podana jako wartość `ConditionScript` atrybutu jest wykonywane w czasie wykonywania. Jeśli ma wartość true, animacji jest wykonywana, w przeciwnym razie nie. Następujące znaczniki zawiera dwa animacji, każdy z nich jest wykonywana w 50% przypadkach na losowe. Ponieważ może istnieć tylko jeden animacji w ramach `<OnLoad>`, dwa `<Condition>` animacji są łączone ze sobą przy użyciu `<Sequence>` elementu:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Należy pamiętać, że znak mniejszości (`<`) w `ConditionScript` atrybut musi być o zmienionym znaczeniu (). Po uruchomieniu tego skryptu, albo brak uruchomień animacji jest jednym z dwóch lub oba są.


[![Panel jest wygaszanie bez zmiany rozmiaru, więc drugiego uruchomienia animacji, pierwszy z nich nie](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

Panel jest wygaszanie bez zmiany rozmiaru, więc drugiego uruchomienia animacji, pierwszy z nich nie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](executing-several-animations-after-each-other-cs.md)
> [dalej](picking-one-animation-out-of-a-list-cs.md)
