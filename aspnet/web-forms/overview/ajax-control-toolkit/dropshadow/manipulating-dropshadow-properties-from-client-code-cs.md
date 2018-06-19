---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulowanie właściwości cień z kodu klienta (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Dostosowywanie interfejsu edycji DataList
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870339"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="d9c48-103">Manipulowanie właściwości cień z kodu klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="d9c48-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="d9c48-104">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d9c48-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d9c48-105">[Pobierz kod](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d9c48-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="d9c48-106">Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień.</span><span class="sxs-lookup"><span data-stu-id="d9c48-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d9c48-107">Właściwości tego rozszerzenia można także zmienić przy użyciu klienta kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d9c48-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="d9c48-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d9c48-108">Overview</span></span>

<span data-ttu-id="d9c48-109">Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień.</span><span class="sxs-lookup"><span data-stu-id="d9c48-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d9c48-110">Właściwości tego rozszerzenia można także zmienić przy użyciu klienta kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d9c48-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="d9c48-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="d9c48-111">Steps</span></span>

<span data-ttu-id="d9c48-112">Kod rozpoczyna się od panelu, zawierającą kilka wierszy tekstu:</span><span class="sxs-lookup"><span data-stu-id="d9c48-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="d9c48-113">Skojarzona klasa CSS daje panelu Kolor tła nieuprzywilejowany:</span><span class="sxs-lookup"><span data-stu-id="d9c48-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="d9c48-114">`DropShadowExtender` Jest dodawany do rozszerzania panel z efektem cienia, nieprzezroczystość ustawiona na 50%:</span><span class="sxs-lookup"><span data-stu-id="d9c48-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="d9c48-115">Następnie ASP.NET AJAX `ScriptManager` formant umożliwia kontroli zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="d9c48-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="d9c48-116">Inny panel zawiera dwa łącza JavaScript do ustawiania nieprzezroczystość cień: minus link zmniejsza nieprzezroczystość w tle, oraz link jej zwiększenie.</span><span class="sxs-lookup"><span data-stu-id="d9c48-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="d9c48-117">Funkcja JavaScript `changeOpacity()` następnie musi najpierw odnaleźć `DropShadowExtender` formantu na stronie.</span><span class="sxs-lookup"><span data-stu-id="d9c48-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="d9c48-118">Definiuje ASP.NET AJAX `$find()` metody dokładnie tego zadania.</span><span class="sxs-lookup"><span data-stu-id="d9c48-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="d9c48-119">Następnie `get_Opacity()` metoda pobiera bieżący nieprzezroczystość `set_Opacity()` metody ustawia ją.</span><span class="sxs-lookup"><span data-stu-id="d9c48-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="d9c48-120">Kod JavaScript następnie przełącza bieżącą wartość nieprzezroczystości `<label>` elementu:</span><span class="sxs-lookup"><span data-stu-id="d9c48-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="d9c48-121">[![Nieprzezroczystość zostanie zmieniona po stronie klienta](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d9c48-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="d9c48-122">Nieprzezroczystość zostanie zmieniona po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d9c48-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d9c48-123">[Poprzednie](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [dalej](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d9c48-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
