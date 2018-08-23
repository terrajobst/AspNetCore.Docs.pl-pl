---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Używanie ogłaszania zwrotnego z kontrolką ReorderList (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki kontrolki ReorderList na zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania. Zawsze, gdy lista jest zmieniana, zamówienia zakupu...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 426e31bc9804c97b551b9c36679d2b821700b915
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755922"
---
<a name="using-postbacks-with-reorderlist-c"></a>Używanie ogłaszania zwrotnego z kontrolką ReorderList (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> Kontrolki kontrolki ReorderList na zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania. Zawsze, gdy lista jest zmieniana, odświeżenie strony informuje serwer zmiany.


## <a name="overview"></a>Omówienie

`ReorderList` Formantu w zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania. Zawsze, gdy lista jest zmieniana, odświeżenie strony informuje serwer zmiany.

## <a name="steps"></a>Kroki

Istnieje kilka źródeł danych dla `ReorderList` kontroli. Jeden jest użycie `XmlDataSource` sterowania:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Aby powiązać plik XML do `ReorderList` musi być ustawiona kontroli i włączenie ogłaszania zwrotnego, następujące atrybuty:

- `DataSourceID`: Identyfikator źródła danych
- `SortOrderField`Sortuj według właściwości
- `AllowReorder`: Czy zezwalać na użytkownika zmienić kolejność elementów listy
- `PostBackOnReorder`: Czy chcesz utworzyć ogłaszania zwrotnego w każdym przypadku, gdy lista jest grupowany

Oto odpowiednie kodu znaczników kontrolki:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

W ramach `ReorderList` kontrolki, określonych danych ze źródła danych można powiązać, przy użyciu `Eval()` metody:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

W dowolnym położeniu na stronie etykiety będzie wyświetlał informacje po ostatniej zmiany kolejności wystąpił:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Ta etykieta jest wypełniany tekst w kodzie po stronie serwera, obsługa ogłaszania zwrotnego:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Na koniec, aby aktywować tę funkcję ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się na stronie:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![Każda zmiana kolejności wyzwala ogłaszania zwrotnego](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Każda zmiana kolejności wyzwala ogłaszania zwrotnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-postbacks-with-reorderlist-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](drag-and-drop-via-reorderlist-cs.md)
