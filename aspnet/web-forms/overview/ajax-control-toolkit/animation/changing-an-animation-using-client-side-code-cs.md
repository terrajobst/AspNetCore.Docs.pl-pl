---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: "Zmiana animacji przy użyciu kodu po stronie klienta (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Animacji można również..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6dcaeac073f54b0804fe3acf7ec22491b1cbbba5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="3372e-104">Zmiana animacji przy użyciu kodu po stronie klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="3372e-104">Changing an Animation Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="3372e-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3372e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3372e-106">[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3372e-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="3372e-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="3372e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3372e-108">Animacji można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3372e-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="3372e-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="3372e-109">Overview</span></span>

<span data-ttu-id="3372e-110">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="3372e-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3372e-111">Animacji można także zmienić przy użyciu niestandardowego kodu JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3372e-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="3372e-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="3372e-112">Steps</span></span>

<span data-ttu-id="3372e-113">Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="3372e-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="3372e-114">Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="3372e-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="3372e-115">Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="3372e-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="3372e-116">Rzeczywiste animacji jest uruchamiany po kliknięciu przycisku kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="3372e-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="3372e-117">Następnie należy dodać `AnimationExtender` ze stroną, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="3372e-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="3372e-118">Należy pamiętać, że ma nie `<Animations>` węzła w `AnimationExtender` formantu.</span><span class="sxs-lookup"><span data-stu-id="3372e-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="3372e-119">Niestandardowy kod JavaScript służy do zapewnienia animacji można używać z formantem.</span><span class="sxs-lookup"><span data-stu-id="3372e-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="3372e-120">Za pomocą interfejsu API serwera z `AnimationExtender`, nie istnieje łatwy sposób można przypisać animacji jeszcze do rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="3372e-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="3372e-121">Jednak extender ujawnia kilka metod do odczytu i zapisu animacji zarejestrowany z różnych zdarzeń (`OnClick`, `OnLoad`i tak dalej).</span><span class="sxs-lookup"><span data-stu-id="3372e-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="3372e-122">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="3372e-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="3372e-123">Format wartość zwracaną `get_*()` funkcje i format argumentu dla `set_*()` funkcji jest ciąg JSON, zawierający reprezentację obiektu jakie byłyby znaczników XML.</span><span class="sxs-lookup"><span data-stu-id="3372e-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="3372e-124">Obecnie nie istnieje sposób do przekazania obiektu, ale istnieje możliwość odczytania obiektu z danym animacji (`get_OnXXXBehavior()` metody).</span><span class="sxs-lookup"><span data-stu-id="3372e-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="3372e-125">Oto ciągu JSON (bez cudzysłowów rozdzielające i dobrze sformatowana) reprezentujący animacji wyzwalane przez naciśnięcie przycisku, ale animacji panelu go i wygaszanie jednocześnie:</span><span class="sxs-lookup"><span data-stu-id="3372e-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="3372e-126">Następujący kod JavaScript przypisuje JSON descripting do `OnClick` animacji bieżącego rozszerzeń i uruchamia go:</span><span class="sxs-lookup"><span data-stu-id="3372e-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


<span data-ttu-id="3372e-127">[![Animacja zostanie uruchomiona natychmiast, bez kliknięcia myszą (i z bardzo mało znaczników)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3372e-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="3372e-128">Animacja zostanie uruchomiona natychmiast, bez kliknięcia myszą (i z bardzo mało znaczników) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3372e-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3372e-129">[Poprzednie](executing-animations-using-client-side-code-cs.md)
[dalej](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="3372e-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
