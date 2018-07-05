---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Zmienianie animacji przy użyciu kodu po stronie klienta (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacji można również...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cc8ca2c962c5ebe5e0c45d5b575031ada3e64acd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386750"
---
<a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="d1d0a-104">Zmienianie animacji przy użyciu kodu po stronie klienta (VB)</span><span class="sxs-lookup"><span data-stu-id="d1d0a-104">Changing an Animation Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="d1d0a-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d1d0a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d1d0a-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d1d0a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="d1d0a-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="d1d0a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d1d0a-108">Animacji można zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d1d0a-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="d1d0a-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d1d0a-109">Overview</span></span>

<span data-ttu-id="d1d0a-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="d1d0a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d1d0a-111">Animacji można zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="d1d0a-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="d1d0a-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="d1d0a-112">Steps</span></span>

<span data-ttu-id="d1d0a-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="d1d0a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="d1d0a-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d1d0a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="d1d0a-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="d1d0a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="d1d0a-116">Rzeczywiste animacji jest uruchamiane po kliknięciu przycisku kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="d1d0a-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="d1d0a-117">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="d1d0a-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="d1d0a-118">Należy pamiętać, że ma nie `<Animations>` węzła w ramach `AnimationExtender` kontroli.</span><span class="sxs-lookup"><span data-stu-id="d1d0a-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="d1d0a-119">Niestandardowy kod JavaScript służy do zapewnienia animacji, który ma być używany z formantem.</span><span class="sxs-lookup"><span data-stu-id="d1d0a-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="d1d0a-120">Podobnie jak w przypadku interfejsu API serwera z `AnimationExtender`, nie istnieje łatwy sposób jeszcze przypisać animacji do urządzenia extender.</span><span class="sxs-lookup"><span data-stu-id="d1d0a-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="d1d0a-121">Jednak urządzenia extender udostępnia kilka metod do odczytu i zapisu animacji zarejestrowane przy użyciu różnych zdarzeń (`OnClick`, `OnLoad`i tak dalej).</span><span class="sxs-lookup"><span data-stu-id="d1d0a-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="d1d0a-122">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="d1d0a-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="d1d0a-123">Format wartości zwracanej z `get_*()` funkcje i format argumentu dla `set_*()` funkcje jest ciągiem JSON, zapewniając znaczników XML, co będzie reprezentacja obiektu.</span><span class="sxs-lookup"><span data-stu-id="d1d0a-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="d1d0a-124">Obecnie nie istnieje sposób do przekazania obiektu w, ale istnieje możliwość odczytania obiektu z danym animacji (`get_OnXXXBehavior()` metody).</span><span class="sxs-lookup"><span data-stu-id="d1d0a-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="d1d0a-125">Oto ciągu JSON (bez cudzysłowu ograniczająca i dobrze sformatowane) reprezentujący animacji wyzwalane po kliknięciu przycisku, ale animowanie panelu, zmienianie jej rozmiaru i wygaszanie w tym samym czasie:</span><span class="sxs-lookup"><span data-stu-id="d1d0a-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="d1d0a-126">Następujący kod JavaScript przypisuje JSON descripting do `OnClick` animacji bieżącego urządzenia extender i uruchamia go:</span><span class="sxs-lookup"><span data-stu-id="d1d0a-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


<span data-ttu-id="d1d0a-127">[![Animacja jest uruchamiany natychmiast, bez kliknięcia (i z niewielkim znaczników)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d1d0a-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="d1d0a-128">Animacja jest uruchamiany natychmiast, bez kliknięcie myszą (i z niewielkim znaczników) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d1d0a-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1d0a-129">[Poprzednie](executing-animations-using-client-side-code-vb.md)
> [dalej](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d1d0a-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
