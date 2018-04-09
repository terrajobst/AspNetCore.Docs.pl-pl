---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Przy użyciu ogłaszania zwrotnego z ReorderList (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrola ReorderList w zestawie narzędzi kontroli AJAX zapewnia listy, które można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania. Zawsze, gdy lista jest kolejności, po...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed01c30c0721c8f1cd8ccb3fea0735ea8fa4f0a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-postbacks-with-reorderlist-c"></a>Przy użyciu ogłaszania zwrotnego z ReorderList (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> Kontrola ReorderList w zestawie narzędzi kontroli AJAX zapewnia listy, które można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania. Zawsze, gdy lista jest ponownie uporządkowane, odświeżenie strony informuje serwer o zmianie.


## <a name="overview"></a>Omówienie

`ReorderList` Formantu w zestawie narzędzi programu AJAX formant zawiera listę można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania. Zawsze, gdy lista jest ponownie uporządkowane, odświeżenie strony informuje serwer o zmianie.

## <a name="steps"></a>Kroki

Istnieje kilka źródeł danych dla `ReorderList` formantu. Jeden jest użycie `XmlDataSource` sterowania:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Aby powiązać to XML do `ReorderList` musi być ustawiona kontroli i Włącz ogłaszania zwrotnego, następujące atrybuty:

- `DataSourceID`: Identyfikator źródła danych
- `SortOrderField`Właściwość, aby posortować według
- `AllowReorder`: Czy zezwolić na użytkownika zmienić kolejność elementów listy
- `PostBackOnReorder`: Czy można utworzyć odświeżenie strony, gdy lista jest grupowany

W tym miejscu jest odpowiedni kod znaczników dla formantu:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

W ramach `ReorderList` kontroli, określone dane ze źródła danych może być powiązany za pomocą `Eval()` metody:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

Od dowolnego pozycji na stronie etykiety będzie wyświetlał informacje po ostatniej zmiany kolejności wystąpił:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Etykieta jest wypełniony tekst w kodzie po stronie serwera, obsługa ogłaszania zwrotnego:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Na koniec, aby aktywować funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się na stronie:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![Każda zmiana kolejności wyzwala odświeżania strony](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Każda zmiana kolejności wyzwala odświeżania strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-postbacks-with-reorderlist-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](drag-and-drop-via-reorderlist-cs.md)
