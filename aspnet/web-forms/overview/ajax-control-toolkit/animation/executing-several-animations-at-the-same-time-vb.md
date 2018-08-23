---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Wykonywanie kilku animacji w tym samym czasie (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Umożliwia uruchamianie severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 20baa40c34dd8c8907b940764987441bc7a91da9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756687"
---
<a name="executing-several-animations-at-the-same-time-vb"></a><span data-ttu-id="661bb-104">Wykonywanie kilku animacji w tym samym czasie (VB)</span><span class="sxs-lookup"><span data-stu-id="661bb-104">Executing Several Animations at The Same Time (VB)</span></span>
====================
<span data-ttu-id="661bb-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="661bb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="661bb-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="661bb-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span></span>

> <span data-ttu-id="661bb-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="661bb-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="661bb-108">Umożliwia uruchamianie kilku animacji w sposób równoległy.</span><span class="sxs-lookup"><span data-stu-id="661bb-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="661bb-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="661bb-109">Overview</span></span>

<span data-ttu-id="661bb-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="661bb-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="661bb-111">Umożliwia uruchamianie kilku animacji w sposób równoległy.</span><span class="sxs-lookup"><span data-stu-id="661bb-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="661bb-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="661bb-112">Steps</span></span>

<span data-ttu-id="661bb-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="661bb-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

<span data-ttu-id="661bb-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="661bb-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

<span data-ttu-id="661bb-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="661bb-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

<span data-ttu-id="661bb-116">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="661bb-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

<span data-ttu-id="661bb-117">W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchamiania animacji, po całkowitym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="661bb-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="661bb-118">Ogólnie rzecz biorąc `<OnLoad>` akceptuje tylko jednej animacji.</span><span class="sxs-lookup"><span data-stu-id="661bb-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="661bb-119">Framework animacji umożliwia Cię do dołączenia do kilku animacji w ją przy użyciu `<Parallel>` elementu.</span><span class="sxs-lookup"><span data-stu-id="661bb-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="661bb-120">Wszystkie animacje w ramach `<Parallel>` są wykonywane w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="661bb-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="661bb-121">Poniżej przedstawiono możliwe kod znaczników dla `AnimationExtender` kontrolki, wygaszanie i zmienianie rozmiaru panelu, w tym samym czasie:</span><span class="sxs-lookup"><span data-stu-id="661bb-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

<span data-ttu-id="661bb-122">I w rzeczywistości: po uruchomieniu tego skryptu, panel jest wyświetlany, a następnie zmienia rozmiar (więcej niż threefolding jego szerokość i halfing wysokość) i stopniowo zmniejsza się w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="661bb-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="661bb-123">[![Panel jest wygaszanie i zmienianie rozmiaru (w tym jego zawartości, dzięki aparat renderowania w przeglądarce)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="661bb-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span></span>

<span data-ttu-id="661bb-124">Panel jest wygaszanie i zmienianie rozmiaru (w tym jego zawartości, dzięki aparat renderowania w przeglądarce) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="661bb-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="661bb-125">[Poprzednie](adding-animation-to-a-control-vb.md)
> [dalej](executing-several-animations-after-each-other-vb.md)</span><span class="sxs-lookup"><span data-stu-id="661bb-125">[Previous](adding-animation-to-a-control-vb.md)
[Next](executing-several-animations-after-each-other-vb.md)</span></span>
