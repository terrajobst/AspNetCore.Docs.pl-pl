---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Wykonywanie animacji w odpowiedzi na interakcję z użytkownikiem (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można w formie gwiazdek...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d9a3bf853afdbde0a419b90d1563fc06d3f9979
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753494"
---
<a name="animating-in-response-to-user-interaction-vb"></a>Wykonywanie animacji w odpowiedzi na interakcję z użytkownikiem (VB)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można uruchomić automatycznie lub mogą być powodowane przez interakcję użytkownika, np. przez kliknięcie myszą.


## <a name="overview"></a>Omówienie

Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można uruchomić automatycznie lub mogą być powodowane przez interakcję użytkownika, np. przez kliknięcie myszą.

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

W ramach `<Animations>` węzła, istnieje pięć sposobów rozpoczęcia animacji za pośrednictwem interakcji z użytkownikiem (Brak elementu jest `<OnLoad>` które jest wykonywane po całkowitym załadowaniu całej strony):

- `<OnClick>` (kliknięcie myszą kontrolki)
- `<OnHoverOut>` (kursora myszy poza formant)
- `<OnHoverOver>` (wskaźnika myszy nad formant zatrzymywanie `<OnHoverOut>` animacji)
- `<OnMouseOut>` (kursora myszy poza formant)
- `<OnMouseOver>` (wskaźnika myszy nad formant nie zatrzymywanie `<OnMouseOut>` animacji)

W tym scenariuszu `<OnClick>` jest używany. Gdy użytkownik kliknie na panelu, rozmiaru i stopniowo zmniejsza się w tym samym czasie.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


[![Kliknięcie myszą powoduje uruchomienie animacji](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

Kliknięcie myszą powoduje uruchomienie animacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](animating-in-response-to-user-interaction-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](picking-one-animation-out-of-a-list-vb.md)
> [dalej](disabling-actions-during-animation-vb.md)
