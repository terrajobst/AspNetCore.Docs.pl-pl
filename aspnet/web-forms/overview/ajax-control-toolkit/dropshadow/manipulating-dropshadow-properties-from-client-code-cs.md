---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulowanie właściwościami DropShadow z poziomu kodu klienta (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Dostosowywanie interfejsu edycji kontrolki DataList
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e3166b9da97a0f4097566b62ba52b6d672eab78f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377288"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="a63d7-103">Manipulowanie właściwościami DropShadow z poziomu kodu klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="a63d7-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="a63d7-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a63d7-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a63d7-105">[Pobierz program Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a63d7-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="a63d7-106">Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia.</span><span class="sxs-lookup"><span data-stu-id="a63d7-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a63d7-107">Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="a63d7-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="a63d7-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="a63d7-108">Overview</span></span>

<span data-ttu-id="a63d7-109">Kontrolki DropShadow na zestawu narzędzi AJAX Control Toolkit rozszerza panelu z cienia.</span><span class="sxs-lookup"><span data-stu-id="a63d7-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a63d7-110">Właściwości tego rozszerzenia można także zmienić przy użyciu kodu JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="a63d7-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a63d7-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="a63d7-111">Steps</span></span>

<span data-ttu-id="a63d7-112">Panel, zawierający kilka wierszy tekstu zaczyna się kod:</span><span class="sxs-lookup"><span data-stu-id="a63d7-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="a63d7-113">Skojarzona klasa CSS zapewnia panelu Kolor tła nieuprzywilejowany:</span><span class="sxs-lookup"><span data-stu-id="a63d7-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="a63d7-114">`DropShadowExtender` Jest dodawany do rozszerzenia panelu z efektem cienia, nieprzezroczystość równa 50%:</span><span class="sxs-lookup"><span data-stu-id="a63d7-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="a63d7-115">Następnie ASP.NET AJAX `ScriptManager` control umożliwia kontrolki zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="a63d7-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="a63d7-116">Kolejny panel zawiera dwa linki JavaScript do ustawiania Nieprzezroczystość cienia: minus link zmniejsza jego nieprzezroczystości, oraz link jej zwiększenie.</span><span class="sxs-lookup"><span data-stu-id="a63d7-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="a63d7-117">Funkcja języka JavaScript `changeOpacity()` następnie musi najpierw odnaleźć `DropShadowExtender` formantu na stronie.</span><span class="sxs-lookup"><span data-stu-id="a63d7-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="a63d7-118">Definiuje ASP.NET AJAX `$find()` metody do dokładnie tego zadania.</span><span class="sxs-lookup"><span data-stu-id="a63d7-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="a63d7-119">Następnie `get_Opacity()` metoda pobiera bieżący nieprzezroczystość `set_Opacity()` metody ustawia ją.</span><span class="sxs-lookup"><span data-stu-id="a63d7-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="a63d7-120">Następnie kod JavaScript umieszcza bieżącą wartość nieprzezroczystości w `<label>` elementu:</span><span class="sxs-lookup"><span data-stu-id="a63d7-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="a63d7-121">[![Nieprzezroczystość zostanie zmieniony po stronie klienta](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a63d7-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="a63d7-122">Nieprzezroczystość zostanie zmieniony po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a63d7-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a63d7-123">[Poprzednie](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [dalej](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a63d7-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
