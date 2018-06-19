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
ms.locfileid: "30871730"
---
<a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="5ff93-104">Przy użyciu ogłaszania zwrotnego z ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="5ff93-104">Using Postbacks with ReorderList (C#)</span></span>
====================
<span data-ttu-id="5ff93-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5ff93-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5ff93-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5ff93-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="5ff93-107">Kontrola ReorderList w zestawie narzędzi kontroli AJAX zapewnia listy, które można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania.</span><span class="sxs-lookup"><span data-stu-id="5ff93-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="5ff93-108">Zawsze, gdy lista jest ponownie uporządkowane, odświeżenie strony informuje serwer o zmianie.</span><span class="sxs-lookup"><span data-stu-id="5ff93-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="5ff93-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5ff93-109">Overview</span></span>

<span data-ttu-id="5ff93-110">`ReorderList` Formantu w zestawie narzędzi programu AJAX formant zawiera listę można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania.</span><span class="sxs-lookup"><span data-stu-id="5ff93-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="5ff93-111">Zawsze, gdy lista jest ponownie uporządkowane, odświeżenie strony informuje serwer o zmianie.</span><span class="sxs-lookup"><span data-stu-id="5ff93-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="5ff93-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="5ff93-112">Steps</span></span>

<span data-ttu-id="5ff93-113">Istnieje kilka źródeł danych dla `ReorderList` formantu.</span><span class="sxs-lookup"><span data-stu-id="5ff93-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="5ff93-114">Jeden jest użycie `XmlDataSource` sterowania:</span><span class="sxs-lookup"><span data-stu-id="5ff93-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="5ff93-115">Aby powiązać to XML do `ReorderList` musi być ustawiona kontroli i Włącz ogłaszania zwrotnego, następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="5ff93-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="5ff93-116">`DataSourceID`: Identyfikator źródła danych</span><span class="sxs-lookup"><span data-stu-id="5ff93-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="5ff93-117">`SortOrderField`Właściwość, aby posortować według</span><span class="sxs-lookup"><span data-stu-id="5ff93-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="5ff93-118">`AllowReorder`: Czy zezwolić na użytkownika zmienić kolejność elementów listy</span><span class="sxs-lookup"><span data-stu-id="5ff93-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="5ff93-119">`PostBackOnReorder`: Czy można utworzyć odświeżenie strony, gdy lista jest grupowany</span><span class="sxs-lookup"><span data-stu-id="5ff93-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="5ff93-120">W tym miejscu jest odpowiedni kod znaczników dla formantu:</span><span class="sxs-lookup"><span data-stu-id="5ff93-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="5ff93-121">W ramach `ReorderList` kontroli, określone dane ze źródła danych może być powiązany za pomocą `Eval()` metody:</span><span class="sxs-lookup"><span data-stu-id="5ff93-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="5ff93-122">Od dowolnego pozycji na stronie etykiety będzie wyświetlał informacje po ostatniej zmiany kolejności wystąpił:</span><span class="sxs-lookup"><span data-stu-id="5ff93-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="5ff93-123">Etykieta jest wypełniony tekst w kodzie po stronie serwera, obsługa ogłaszania zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="5ff93-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="5ff93-124">Na koniec, aby aktywować funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się na stronie:</span><span class="sxs-lookup"><span data-stu-id="5ff93-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


<span data-ttu-id="5ff93-125">[![Każda zmiana kolejności wyzwala odświeżania strony](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5ff93-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="5ff93-126">Każda zmiana kolejności wyzwala odświeżania strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5ff93-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5ff93-127">Next</span><span class="sxs-lookup"><span data-stu-id="5ff93-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
