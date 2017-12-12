---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Wykonywanie kilku animacji, w tym samym czasie (C#) | Dokumentacja firmy Microsoft
author: wenz
description: "Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Umożliwia uruchamianie severa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: b7ee9c2da392bed512e259b44237e5ff997dca23
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-at-the-same-time-c"></a>Wykonywanie kilku animacji, w tym samym czasie (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Umożliwia uruchamianie kilka animacji w sposób równoległy.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Umożliwia uruchamianie kilka animacji w sposób równoległy.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchomienia animacji po całkowitym załadowaniem strony. Ogólnie rzecz biorąc `<OnLoad>` akceptuje tylko jednej animacji. Framework animacji można sprzęgać kilka animacji w jednej przy użyciu `<Parallel>` elementu. Wszystkie animacje w `<Parallel>` są wykonywane w tym samym czasie.

Oto możliwe kod znaczników dla `AnimationExtender` kontroli, wygaszanie i zmienianie rozmiaru panelu, w tym samym czasie:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

A w rzeczywistości: po uruchomieniu tego skryptu panelu jest wyświetlane, a następnie zmienia rozmiar (więcej niż threefolding szerokości i halfing wysokość) i stopniowo zmniejsza się w tym samym czasie.


[![Panel jest wygaszanie i zmiany rozmiaru (w tym jego zawartości, dzięki użyciu aparatu renderowania w przeglądarce)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

Panel jest wygaszanie i zmiany rozmiaru (w tym jego zawartości, dzięki użyciu aparatu renderowania w przeglądarce) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-at-the-same-time-cs/_static/image3.png))

>[!div class="step-by-step"]
[Poprzednie](adding-animation-to-a-control-cs.md)
[dalej](executing-several-animations-after-each-other-cs.md)
