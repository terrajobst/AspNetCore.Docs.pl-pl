---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modyfikowanie animacji po stronie serwera (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacje mogą również...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 83ce54b1cd2c226db36be75f61321a0fb710e0ca
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376665"
---
<a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="d32bb-104">Modyfikowanie animacji po stronie serwera (C#)</span><span class="sxs-lookup"><span data-stu-id="d32bb-104">Modifying Animations From The Server Side (C#)</span></span>
====================
<span data-ttu-id="d32bb-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d32bb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d32bb-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d32bb-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="d32bb-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="d32bb-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d32bb-108">Może także ulec animacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="d32bb-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="d32bb-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d32bb-109">Overview</span></span>

<span data-ttu-id="d32bb-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="d32bb-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d32bb-111">Może także ulec animacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="d32bb-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="d32bb-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="d32bb-112">Steps</span></span>

<span data-ttu-id="d32bb-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="d32bb-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="d32bb-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d32bb-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="d32bb-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="d32bb-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="d32bb-116">Pozostała część kodu jest uruchamiany po stronie serwera i nie używa znaczników; Zamiast tego używa kodu do tworzenia `AnimationExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="d32bb-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="d32bb-117">Jednak zestaw narzędzi kontroli aktualnie nie zapewnia dostęp do interfejsu API do tworzenia indywidualnych animacji.</span><span class="sxs-lookup"><span data-stu-id="d32bb-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="d32bb-118">Jednak jest możliwe ustawienie `AnimationExtender`właściwości animacji z ciągiem zawierającym znaczników XML używane podczas przypisywania animacji w sposób deklaratywny.</span><span class="sxs-lookup"><span data-stu-id="d32bb-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="d32bb-119">Aby można było utworzyć plik XML, który nie może zawierać `<Animations>` elementu, można użyć XML .NET Framework obsługuje lub zgodnie z poniższym kodem, po prostu podać ciąg:</span><span class="sxs-lookup"><span data-stu-id="d32bb-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="d32bb-120">Na koniec należy dodać `AnimationExtender` sterowania do bieżącej strony, w ramach `<form runat="server">` elementu, upewniając się, że animacja jest dołączony i uruchamia:</span><span class="sxs-lookup"><span data-stu-id="d32bb-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


<span data-ttu-id="d32bb-121">[![Animacja zostanie utworzony przy użyciu języka C# /VB kod na serwerze](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d32bb-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="d32bb-122">Animacja zostanie utworzony przy użyciu języka C# /VB kod na serwerze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d32bb-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d32bb-123">[Poprzednie](triggering-an-animation-in-another-control-cs.md)
> [dalej](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d32bb-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
