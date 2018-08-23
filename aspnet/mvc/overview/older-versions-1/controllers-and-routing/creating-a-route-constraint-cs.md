---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Tworzenie ograniczenia trasy (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'W tym samouczku Walther Autor: Stephen pokazuje, jak kontrolować, jak przeglądarka żąda dopasowanie tras przez tworzenie ograniczenia trasy z wyrażeniami regularnymi.'
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51d76248a59968e1d6befd5d5404f7bf6c2878e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753838"
---
<a name="creating-a-route-constraint-c"></a>Tworzenie ograniczenia trasy (C#)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> W tym samouczku Walther Autor: Stephen pokazuje, jak kontrolować, jak przeglądarka żąda dopasowanie tras przez tworzenie ograniczenia trasy z wyrażeniami regularnymi.


Ograniczenia trasy umożliwia ograniczanie żądań przeglądarki, które pasuje do określonej trasy. Aby określić ograniczenia trasy, można użyć wyrażenia regularnego.

Na przykład Wyobraź sobie, że zdefiniowano trasy w ofercie 1 w pliku Global.asax.

**Wyświetlanie listy 1 — Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

Wyświetlanie listy 1 zawiera trasę o nazwie produktu. Trasy produktu służy do mapowania żądania przeglądarki ProductController zawarte w ofercie 2.

**Wyświetlanie listy 2 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Należy zauważyć, że akcja Details() udostępnianych przez kontroler produktu przyjmuje jeden parametr o nazwie productId. Ten parametr jest parametrem liczby całkowitej.

Trasy zdefiniowane w ofercie 1 będzie pasuje do żadnego z następujących adresów URL:

- / Produktu/23
- Produkt/7 dni w tygodniu

Niestety trasy się zgadzać następujące adresy URL:

- / Produktu/blah
- / Produktu/firmy apple

Ponieważ akcji Details() oczekuje parametru liczby całkowitej, żądania zawiera coś innego niż wartość całkowitą spowoduje błąd. Na przykład jeśli wpiszesz /Product/apple adresu URL w przeglądarce następnie otrzymasz strony błędu na rysunku 1.


[![Okno dialogowe Nowy projekt](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Rysunek 01**: wyświetlanie strony explode ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-route-constraint-cs/_static/image2.png))


Co tak naprawdę chcesz zrobić to dopasowanie tylko adresów URL zawierających productId właściwej liczby całkowitej. Można użyć ograniczenie podczas definiowania trasy, aby ograniczyć adresy URL, które odpowiadają trasy. Zmodyfikowane trasy produktu w ofercie 3 zawiera ograniczenia wyrażenia regularnego, który jest zgodny tylko liczby całkowite.

**Wyświetlanie listy 3 — Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

\D+ wyrażenia regularnego dopasowuje jeden lub więcej liczb całkowitych. To ograniczenie powoduje, że trasy produktu dopasować następujące adresy URL:

- / Produkt/3
- / Produktu/8999

Ale nie następujące adresy URL:

- / Produktu/firmy apple
- / Produktu

- Te żądania przeglądarki będzie obsługiwany przez innej trasy lub, jeśli istnieje nie zgodnych tras *nie można odnaleźć zasobu* zostanie zwrócony błąd.

> [!div class="step-by-step"]
> [Poprzednie](creating-custom-routes-cs.md)
> [dalej](creating-a-custom-route-constraint-cs.md)
