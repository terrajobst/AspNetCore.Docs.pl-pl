---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Wykonywanie kilku animacji jedna po drugiej (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Umożliwia uruchamianie severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d9e1ce0c156752ce0e97b91f253ced26360a721
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364756"
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="54259-104">Wykonywanie kilku animacji jedna po drugiej (VB)</span><span class="sxs-lookup"><span data-stu-id="54259-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="54259-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="54259-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="54259-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="54259-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="54259-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="54259-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="54259-108">Umożliwia uruchamianie kilku animacji jedna po drugiej.</span><span class="sxs-lookup"><span data-stu-id="54259-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="54259-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="54259-109">Overview</span></span>

<span data-ttu-id="54259-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="54259-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="54259-111">Umożliwia uruchamianie kilku animacji jedna po drugiej.</span><span class="sxs-lookup"><span data-stu-id="54259-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="54259-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="54259-112">Steps</span></span>

<span data-ttu-id="54259-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="54259-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="54259-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="54259-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="54259-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="54259-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="54259-116">Następnie należy dodać `AnimationExtender` do strony, zapewniając `ID`, `TargetControlID` atrybut i obowiązkowe `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="54259-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="54259-117">W ramach `<Animations>` węzła, użyj `<OnLoad>` do uruchamiania animacji, po całkowitym załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="54259-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="54259-118">Ogólnie rzecz biorąc `<OnLoad>` akceptuje tylko jednej animacji.</span><span class="sxs-lookup"><span data-stu-id="54259-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="54259-119">Framework animacji umożliwia Cię do dołączenia do kilku animacji w ją przy użyciu `<Sequence>` elementu.</span><span class="sxs-lookup"><span data-stu-id="54259-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="54259-120">Wszystkie animacje w ramach `<Sequence>` są wykonane jeden po drugim.</span><span class="sxs-lookup"><span data-stu-id="54259-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="54259-121">Poniżej przedstawiono możliwe kod znaczników dla `AnimationExtender` kontrolki, najpierw dokonując panelu szerszy i następnie zmniejszenie jego wysokość:</span><span class="sxs-lookup"><span data-stu-id="54259-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="54259-122">Po uruchomieniu tego skryptu panelu otrzymuje szerszy i następnie mniejsze.</span><span class="sxs-lookup"><span data-stu-id="54259-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="54259-123">[![Najpierw jest zwiększana szerokość](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="54259-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="54259-124">Najpierw jest zwiększana szerokość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="54259-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="54259-125">[![Zmniejszy wysokość](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="54259-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="54259-126">Następnie zmniejszyła się wysokość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="54259-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="54259-127">[Poprzednie](executing-several-animations-at-the-same-time-vb.md)
> [dalej](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="54259-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
