---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Zmienianie animacji przy użyciu kodu po stronie klienta (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można również...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 253377ef6019a672680c6e819349357627ef111b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756152"
---
<a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="253bb-104">Zmienianie animacji przy użyciu kodu po stronie klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="253bb-104">Changing an Animation Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="253bb-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="253bb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="253bb-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="253bb-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="253bb-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="253bb-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="253bb-108">Animacji można zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="253bb-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="253bb-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="253bb-109">Overview</span></span>

<span data-ttu-id="253bb-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="253bb-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="253bb-111">Animacji można zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="253bb-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="253bb-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="253bb-112">Steps</span></span>

<span data-ttu-id="253bb-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="253bb-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="253bb-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="253bb-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="253bb-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="253bb-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="253bb-116">Rzeczywiste animacji jest uruchamiane po kliknięciu przycisku kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="253bb-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="253bb-117">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="253bb-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="253bb-118">Należy pamiętać, że ma nie `<Animations>` węzła w ramach `AnimationExtender` kontroli.</span><span class="sxs-lookup"><span data-stu-id="253bb-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="253bb-119">Niestandardowy kod JavaScript służy do zapewnienia animacji, który ma być używany z formantem.</span><span class="sxs-lookup"><span data-stu-id="253bb-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="253bb-120">Podobnie jak w przypadku interfejsu API serwera z `AnimationExtender`, nie istnieje łatwy sposób jeszcze przypisać animacji do urządzenia extender.</span><span class="sxs-lookup"><span data-stu-id="253bb-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="253bb-121">Jednak urządzenia extender udostępnia kilka metod do odczytu i zapisu animacji zarejestrowane przy użyciu różnych zdarzeń (`OnClick`, `OnLoad`i tak dalej).</span><span class="sxs-lookup"><span data-stu-id="253bb-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="253bb-122">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="253bb-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="253bb-123">Format wartości zwracanej z `get_*()` funkcje i format argumentu dla `set_*()` funkcje jest ciągiem JSON, zapewniając znaczników XML, co będzie reprezentacja obiektu.</span><span class="sxs-lookup"><span data-stu-id="253bb-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="253bb-124">Obecnie nie istnieje sposób do przekazania obiektu w, ale istnieje możliwość odczytania obiektu z danym animacji (`get_OnXXXBehavior()` metody).</span><span class="sxs-lookup"><span data-stu-id="253bb-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="253bb-125">Oto ciągu JSON (bez cudzysłowu ograniczająca i dobrze sformatowane) reprezentujący animacji wyzwalane po kliknięciu przycisku, ale animowanie panelu, zmienianie jej rozmiaru i wygaszanie w tym samym czasie:</span><span class="sxs-lookup"><span data-stu-id="253bb-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="253bb-126">Następujący kod JavaScript przypisuje JSON descripting do `OnClick` animacji bieżącego urządzenia extender i uruchamia go:</span><span class="sxs-lookup"><span data-stu-id="253bb-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


<span data-ttu-id="253bb-127">[![Animacja jest uruchamiany natychmiast, bez kliknięcia (i z niewielkim znaczników)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="253bb-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="253bb-128">Animacja jest uruchamiany natychmiast, bez kliknięcie myszą (i z niewielkim znaczników) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="253bb-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="253bb-129">[Poprzednie](executing-animations-using-client-side-code-cs.md)
> [dalej](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="253bb-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
