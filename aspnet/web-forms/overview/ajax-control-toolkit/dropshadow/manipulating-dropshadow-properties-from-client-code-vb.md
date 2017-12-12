---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: "Manipulowanie właściwości cień z kodu klienta (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień. Właściwości tego rozszerzenia można zmienić za pomocą klienta JavaScrip..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 706ccb5a95873aad4c1b9e0942ab06cf4162710a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="d468f-104">Manipulowanie właściwości cień z kodu klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="d468f-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="d468f-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d468f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d468f-106">[Pobierz kod](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d468f-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="d468f-107">Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień.</span><span class="sxs-lookup"><span data-stu-id="d468f-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d468f-108">Właściwości tego rozszerzenia można także zmienić przy użyciu klienta kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d468f-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="d468f-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d468f-109">Overview</span></span>

<span data-ttu-id="d468f-110">Formant cień w zestawie narzędzi kontroli AJAX rozszerza panel z cień.</span><span class="sxs-lookup"><span data-stu-id="d468f-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d468f-111">Właściwości tego rozszerzenia można także zmienić przy użyciu klienta kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d468f-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="d468f-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="d468f-112">Steps</span></span>

<span data-ttu-id="d468f-113">Kod rozpoczyna się od panelu, zawierającą kilka wierszy tekstu:</span><span class="sxs-lookup"><span data-stu-id="d468f-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="d468f-114">Skojarzona klasa CSS daje panelu Kolor tła nieuprzywilejowany:</span><span class="sxs-lookup"><span data-stu-id="d468f-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="d468f-115">`DropShadowExtender` Jest dodawany do rozszerzania panel z efektem cienia, nieprzezroczystość ustawiona na 50%:</span><span class="sxs-lookup"><span data-stu-id="d468f-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="d468f-116">Następnie ASP.NET AJAX `ScriptManager` formant umożliwia kontroli zestawu narzędzi do pracy:</span><span class="sxs-lookup"><span data-stu-id="d468f-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="d468f-117">Inny panel zawiera dwa łącza JavaScript do ustawiania nieprzezroczystość cień: minus link zmniejsza nieprzezroczystość w tle, oraz link jej zwiększenie.</span><span class="sxs-lookup"><span data-stu-id="d468f-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="d468f-118">Funkcja JavaScript `changeOpacity()` następnie musi najpierw odnaleźć `DropShadowExtender` formantu na stronie.</span><span class="sxs-lookup"><span data-stu-id="d468f-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="d468f-119">Definiuje ASP.NET AJAX `$find()` metody dokładnie tego zadania.</span><span class="sxs-lookup"><span data-stu-id="d468f-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="d468f-120">Następnie `get_Opacity()` metoda pobiera bieżący nieprzezroczystość `set_Opacity()` metody ustawia ją.</span><span class="sxs-lookup"><span data-stu-id="d468f-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="d468f-121">Kod JavaScript następnie przełącza bieżącą wartość nieprzezroczystości `<label>` elementu:</span><span class="sxs-lookup"><span data-stu-id="d468f-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="d468f-122">[![Nieprzezroczystość zostanie zmieniona po stronie klienta](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d468f-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="d468f-123">Nieprzezroczystość zostanie zmieniona po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d468f-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d468f-124">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="d468f-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
