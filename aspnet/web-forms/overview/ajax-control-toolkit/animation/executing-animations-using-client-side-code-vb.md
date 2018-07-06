---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Wykonywanie animacji przy użyciu kodu po stronie klienta (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Wykonywanie animacji...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b180ad17e0e3d2dffa6262d10a83a8353e0a1d0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811889"
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="45afa-104">Wykonywanie animacji przy użyciu kodu po stronie klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="45afa-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="45afa-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="45afa-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="45afa-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="45afa-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="45afa-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="45afa-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="45afa-108">Wykonywanie animacji może być też wywoływane przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="45afa-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="45afa-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="45afa-109">Overview</span></span>

<span data-ttu-id="45afa-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="45afa-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="45afa-111">Wykonywanie animacji może być też wywoływane przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="45afa-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="45afa-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="45afa-112">Steps</span></span>

<span data-ttu-id="45afa-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="45afa-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="45afa-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="45afa-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="45afa-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="45afa-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="45afa-116">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="45afa-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="45afa-117">W ramach `<Animations>` węzła, użyj `<OnClick>` do uruchamiania animacji po użytkownik klika przycisk na panel.</span><span class="sxs-lookup"><span data-stu-id="45afa-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="45afa-118">Dodaj dwa animacji do wykonania parallelly:</span><span class="sxs-lookup"><span data-stu-id="45afa-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="45afa-119">Celach demonstracyjnych ta animacja (i inne animacje utworzone przy użyciu zestawu narzędzi kontroli) jest wykonywane przy użyciu kodu JavaScript, po uruchomieniu na stronie.</span><span class="sxs-lookup"><span data-stu-id="45afa-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="45afa-120">Po pierwsze: konieczny jest dostęp do `AnimationExtender` kontroli.</span><span class="sxs-lookup"><span data-stu-id="45afa-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="45afa-121">Biblioteka rozszerzeń ASP.NET AJAX zapewnia `$find()` funkcji dla tego zadania:</span><span class="sxs-lookup"><span data-stu-id="45afa-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="45afa-122">`AnimationExtender` Kontroli udostępnia zaawansowane interfejsy API, włączając w to metody z nazwami identyczne do obsługi zdarzeń, używany w znaczników XML: `OnClick()`, `OnLoad()`i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="45afa-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="45afa-123">Na przykład, wywołanie `OnClick()` metoda jest wykonywana animacji w ramach `<OnClick>` elementu `AnimationExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="45afa-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="45afa-124">Oto kompletny kod JavaScript po stronie klienta, który symuluje kliknij w panelu po całkowitym załadowaniu strony, należy pamiętać, że `pageLoad()` używana jest nazwa funkcji, które jest wywoływane przez ASP.NET AJAX po stronie, a wszystkie dołączone zostały bibliotek JavaScript załadowane.</span><span class="sxs-lookup"><span data-stu-id="45afa-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="45afa-125">[![Animacja jest uruchamiany natychmiast, bez kliknięcia myszą](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="45afa-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="45afa-126">Animacja jest uruchamiany natychmiast, bez kliknięcie myszą ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="45afa-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="45afa-127">[Poprzednie](modifying-animations-from-the-server-side-vb.md)
> [dalej](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="45afa-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
