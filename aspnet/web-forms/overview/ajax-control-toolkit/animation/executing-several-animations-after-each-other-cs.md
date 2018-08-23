---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Wykonywanie kilku animacji jedna po drugiej (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Umożliwia uruchamianie severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 60fb7dacfe59eaafef71fcc9cde61b7840624343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754236"
---
<a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="ea372-104">Wykonywanie kilku animacji jedna po drugiej (C#)</span><span class="sxs-lookup"><span data-stu-id="ea372-104">Executing Several Animations after Each Other (C#)</span></span>
====================
<span data-ttu-id="ea372-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ea372-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ea372-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ea372-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="ea372-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="ea372-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ea372-108">Umożliwia uruchamianie kilku animacji jedna po drugiej.</span><span class="sxs-lookup"><span data-stu-id="ea372-108">It allows to run several animations one after the other.</span></span>


<span data-ttu-id="ea372-109">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="ea372-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ea372-110">Umożliwia uruchamianie kilku animacji jedna po drugiej.</span><span class="sxs-lookup"><span data-stu-id="ea372-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="ea372-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="ea372-111">Steps</span></span>

<span data-ttu-id="ea372-112">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="ea372-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="ea372-113">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ea372-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="ea372-114">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="ea372-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="ea372-115">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="ea372-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="ea372-116">W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchamiania animacji, po całkowitym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="ea372-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="ea372-117">Ogólnie rzecz biorąc `<OnLoad>` akceptuje tylko jednej animacji.</span><span class="sxs-lookup"><span data-stu-id="ea372-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="ea372-118">Framework animacji umożliwia Cię do dołączenia do kilku animacji w ją przy użyciu `<Sequence>` elementu.</span><span class="sxs-lookup"><span data-stu-id="ea372-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="ea372-119">Wszystkie animacje w ramach `<Sequence>` są wykonane jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="ea372-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="ea372-120">Poniżej przedstawiono możliwe kod znaczników dla `AnimationExtender` kontrolki, najpierw dokonując panelu szerszy i następnie zmniejszenie jego wysokość:</span><span class="sxs-lookup"><span data-stu-id="ea372-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="ea372-121">Po uruchomieniu tego skryptu panelu otrzymuje szerszy i następnie mniejsze.</span><span class="sxs-lookup"><span data-stu-id="ea372-121">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="ea372-122">[![Najpierw jest zwiększana szerokość](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ea372-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="ea372-123">Najpierw jest zwiększana szerokość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ea372-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>


<span data-ttu-id="ea372-124">[![Zmniejszy wysokość](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ea372-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="ea372-125">Następnie zmniejszyła się wysokość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ea372-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ea372-126">[Poprzednie](executing-several-animations-at-the-same-time-cs.md)
> [dalej](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ea372-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
