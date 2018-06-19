---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Przy użyciu ogłaszania zwrotnego z ReorderList (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrola ReorderList w zestawie narzędzi kontroli AJAX zapewnia listy, które można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania. Zawsze, gdy lista jest kolejności, po...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: ef43471f7d8cc94c1a82a368e27acef05f474f81
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879494"
---
<a name="using-postbacks-with-reorderlist-vb"></a>Przy użyciu ogłaszania zwrotnego z ReorderList (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> Kontrola ReorderList w zestawie narzędzi kontroli AJAX zapewnia listy, które można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania. Zawsze, gdy lista jest ponownie uporządkowane, odświeżenie strony informuje serwer o zmianie.


## <a name="overview"></a>Omówienie

`ReorderList` Formantu w zestawie narzędzi programu AJAX formant zawiera listę można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania. Zawsze, gdy lista jest ponownie uporządkowane, odświeżenie strony informuje serwer o zmianie.

## <a name="steps"></a>Kroki

Istnieje kilka źródeł danych dla `ReorderList` formantu. Jeden jest użycie `XmlDataSource` sterowania:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Aby powiązać to XML do `ReorderList` musi być ustawiona kontroli i Włącz ogłaszania zwrotnego, następujące atrybuty:

- `DataSourceID`: Identyfikator źródła danych
- `SortOrderField`Właściwość, aby posortować według
- `AllowReorder`: Czy zezwolić na użytkownika zmienić kolejność elementów listy
- `PostBackOnReorder`: Czy można utworzyć odświeżenie strony, gdy lista jest grupowany

W tym miejscu jest odpowiedni kod znaczników dla formantu:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

W ramach `ReorderList` kontroli, określone dane ze źródła danych może być powiązany za pomocą `Eval()` metody:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

Od dowolnego pozycji na stronie etykiety będzie wyświetlał informacje po ostatniej zmiany kolejności wystąpił:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Etykieta jest wypełniony tekst w kodzie po stronie serwera, obsługa ogłaszania zwrotnego:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Na koniec, aby aktywować funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się na stronie:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![Każda zmiana kolejności wyzwala odświeżania strony](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Każda zmiana kolejności wyzwala odświeżania strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](drag-and-drop-via-reorderlist-cs.md)
> [dalej](drag-and-drop-via-reorderlist-vb.md)
