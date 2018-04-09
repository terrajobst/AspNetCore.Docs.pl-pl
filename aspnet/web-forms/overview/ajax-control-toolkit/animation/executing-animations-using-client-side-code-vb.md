---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Wykonywanie animacji przy użyciu kodu po stronie klienta (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Wykonanie animacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: e2b8c499edaccc70d439a576feb80e91cb28c1c7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="71e23-104">Wykonywanie animacji przy użyciu kodu po stronie klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="71e23-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="71e23-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="71e23-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="71e23-106">[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="71e23-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="71e23-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="71e23-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="71e23-108">Wykonanie animacji również mogą być wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="71e23-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="71e23-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="71e23-109">Overview</span></span>

<span data-ttu-id="71e23-110">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="71e23-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="71e23-111">Wykonanie animacji również mogą być wyzwalane przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="71e23-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="71e23-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="71e23-112">Steps</span></span>

<span data-ttu-id="71e23-113">Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="71e23-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="71e23-114">Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="71e23-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="71e23-115">Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="71e23-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="71e23-116">Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="71e23-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="71e23-117">W ramach `<Animations>` węzła, użyj `<OnClick>` jednorazowego uruchomienia animacji użytkownik kliknie na panelu.</span><span class="sxs-lookup"><span data-stu-id="71e23-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="71e23-118">Dodaj dwa animacji można wykonać parallelly:</span><span class="sxs-lookup"><span data-stu-id="71e23-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="71e23-119">Dla celów demonstracyjnych to animacji (i innych animacji utworzone przy użyciu zestawu narzędzi kontroli) jest wykonywana przy użyciu kodu JavaScript, po uruchomieniu strony.</span><span class="sxs-lookup"><span data-stu-id="71e23-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="71e23-120">Przede wszystkim należy dostęp do `AnimationExtender` formantu.</span><span class="sxs-lookup"><span data-stu-id="71e23-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="71e23-121">Biblioteka ASP.NET AJAX zapewnia `$find()` funkcji dla tego zadania:</span><span class="sxs-lookup"><span data-stu-id="71e23-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="71e23-122">`AnimationExtender` Kontroli przedstawia sformatowanego interfejsu API, tym metody z nazwami identyczne z obsługi zdarzeń, używany w znaczniku XML: `OnClick()`, `OnLoad()`i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="71e23-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="71e23-123">Na przykład wywołanie elementu `OnClick()` metoda wykonuje animacji w `<OnClick>` elementu `AnimationExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="71e23-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="71e23-124">W tym miejscu jest kompletny kod JavaScript po stronie klienta, która emuluje kliknij na panelu po całkowitym załadowaniem strony, należy pamiętać, że `pageLoad()` używana jest nazwa funkcji, która jest wywoływana przez ASP.NET AJAX po stronie i wszystkie zawarte zostały biblioteki JavaScript załadowane.</span><span class="sxs-lookup"><span data-stu-id="71e23-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="71e23-125">[![Animacja zostanie uruchomiona natychmiast, bez kliknięcia myszą](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="71e23-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="71e23-126">Animacja zostanie uruchomiona natychmiast, bez kliknięcia myszą ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="71e23-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="71e23-127">[Poprzednie](modifying-animations-from-the-server-side-vb.md)
> [dalej](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="71e23-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
