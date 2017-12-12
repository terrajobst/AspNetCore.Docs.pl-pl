---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: "Animowanie w odpowiedzi na interakcję użytkownika (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Animacji można w formie gwiazdek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: efb9c34c317ec56b43c498f40a857a9b47fa50b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="animating-in-response-to-user-interaction-c"></a>Animowanie w odpowiedzi na interakcję użytkownika (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Animacji można uruchomić automatycznie lub mogą być wyzwalane przez użytkownika, np. przez kliknięcie myszą.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Animacji można uruchomić automatycznie lub mogą być wyzwalane przez użytkownika, np. przez kliknięcie myszą.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

W ramach `<Animations>` węzła, istnieją pięć sposoby rozpoczęcia animacji za pośrednictwem interakcji z użytkownikiem (Brak elementu jest `<OnLoad>` który jest wykonywany po całkowitym załadowaniem całej strony):

- `<OnClick>`(kliknięcie myszą kontrolki)
- `<OnHoverOut>`(mysz opuści formantu)
- `<OnHoverOver>`(przesuwania wskaźnika myszy nad formantem zatrzymywanie `<OnHoverOut>` animacji)
- `<OnMouseOut>`(mysz opuści formantu)
- `<OnMouseOver>`(przesuwania wskaźnika myszy nad formantem nie zatrzymywanie `<OnMouseOut>` animacji)

W tym scenariuszu `<OnClick>` jest używany. Gdy użytkownik kliknie na panelu, rozmiaru i stopniowo zmniejsza się w tym samym czasie.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


[![Kliknięcie powoduje uruchomienie animacji](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Kliknięcie powoduje uruchomienie animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-in-response-to-user-interaction-cs/_static/image3.png))

>[!div class="step-by-step"]
[Poprzednie](picking-one-animation-out-of-a-list-cs.md)
[dalej](disabling-actions-during-animation-cs.md)
