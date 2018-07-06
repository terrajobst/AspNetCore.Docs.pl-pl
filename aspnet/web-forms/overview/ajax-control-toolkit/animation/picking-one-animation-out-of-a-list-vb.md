---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Wybieranie jednej animacji spoza listy (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Struktura również zez...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 126f1b03897763f0619f893d23ab2e763206d08e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809890"
---
<a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="c16d1-104">Wybieranie jednej animacji spoza listy (VB)</span><span class="sxs-lookup"><span data-stu-id="c16d1-104">Picking One Animation Out Of a List (VB)</span></span>
====================
<span data-ttu-id="c16d1-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c16d1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c16d1-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c16d1-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="c16d1-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="c16d1-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c16d1-108">Struktura umożliwia także programisty do pobrania jednej animacji spoza listy animacji, w zależności od oceny kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c16d1-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="c16d1-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c16d1-109">Overview</span></span>

<span data-ttu-id="c16d1-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="c16d1-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c16d1-111">Struktura umożliwia także programisty do pobrania jednej animacji spoza listy animacji, w zależności od oceny kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c16d1-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="c16d1-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="c16d1-112">Steps</span></span>

<span data-ttu-id="c16d1-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="c16d1-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="c16d1-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c16d1-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="c16d1-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="c16d1-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="c16d1-116">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="c16d1-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="c16d1-117">W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchamiania animacji, po całkowitym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="c16d1-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="c16d1-118">Zamiast jednej z regularnych animacji `<Case>` element, który jest dostarczany do gry.</span><span class="sxs-lookup"><span data-stu-id="c16d1-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="c16d1-119">Wartość jego atrybutu SelectScript jest oceniany; zwracana wartość musi być numeryczny.</span><span class="sxs-lookup"><span data-stu-id="c16d1-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="c16d1-120">W zależności od tego numeru, jeden subanimations w ramach &lt;przypadek&gt; jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="c16d1-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="c16d1-121">Na przykład jeśli SelectScript ma wartość 2, zestaw narzędzi kontroli uruchamia trzeci animacji w ramach &lt;przypadek&gt; (zliczanie rozpoczyna się od 0).</span><span class="sxs-lookup"><span data-stu-id="c16d1-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="c16d1-122">Następujące znaczniki definiuje trzy subanimations: zmiana rozmiaru szerokość, wysokość rozmiaru i wygaszanie. Kod JavaScript (`Math.floor(3 * Math.random())`) następnie wybiera liczbą z zakresu od 0 do 2, tak, aby jeden z trzech animacji uruchomieniu:</span><span class="sxs-lookup"><span data-stu-id="c16d1-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


<span data-ttu-id="c16d1-123">[![Jeden z trzech możliwych animacji: poszerzania panelu](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c16d1-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="c16d1-124">Jeden z trzech możliwych animacji: poszerzania panelu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c16d1-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c16d1-125">[Poprzednie](animation-depending-on-a-condition-vb.md)
> [dalej](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c16d1-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
