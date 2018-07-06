---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modyfikowanie animacji po stronie serwera (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki. Animacje mogą również...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ff8685b44e7d0c2261ea7b9f1ca6397ae05ce53
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811169"
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="8ab46-104">Modyfikowanie animacji po stronie serwera (VB)</span><span class="sxs-lookup"><span data-stu-id="8ab46-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="8ab46-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8ab46-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8ab46-106">[Pobierz program Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8ab46-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="8ab46-107">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="8ab46-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8ab46-108">Może także ulec animacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="8ab46-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="8ab46-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="8ab46-109">Overview</span></span>

<span data-ttu-id="8ab46-110">Kontrolki animacji w programie ASP.NET AJAX Control Toolkit nie jest po prostu kontrolki, ale cała struktura Dodawanie animacji do kontrolki.</span><span class="sxs-lookup"><span data-stu-id="8ab46-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8ab46-111">Może także ulec animacji po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="8ab46-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="8ab46-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="8ab46-112">Steps</span></span>

<span data-ttu-id="8ab46-113">Po pierwsze, obejmują `ScriptManager` w strony, a następnie biblioteki ASP.NET AJAX jest ładowany, dzięki czemu można użyć zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="8ab46-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="8ab46-114">Animacja zostanie zastosowana do panelu tekstu, który wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8ab46-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="8ab46-115">W skojarzone klasy CSS do obsługi panelu zdefiniowany jako kolor tła dobre rozwiązanie, a także ustawić stała szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="8ab46-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="8ab46-116">Pozostała część kodu jest uruchamiany po stronie serwera i nie używa znaczników; Zamiast tego używa kodu do tworzenia `AnimationExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="8ab46-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="8ab46-117">Jednak zestaw narzędzi kontroli aktualnie nie zapewnia dostęp do interfejsu API do tworzenia indywidualnych animacji.</span><span class="sxs-lookup"><span data-stu-id="8ab46-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="8ab46-118">Jednak jest możliwe ustawienie `AnimationExtender`właściwości animacji z ciągiem zawierającym znaczników XML używane podczas przypisywania animacji w sposób deklaratywny.</span><span class="sxs-lookup"><span data-stu-id="8ab46-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="8ab46-119">Aby można było utworzyć plik XML, który nie może zawierać `<Animations>` elementu, można użyć XML .NET Framework obsługuje lub zgodnie z poniższym kodem, po prostu podać ciąg:</span><span class="sxs-lookup"><span data-stu-id="8ab46-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="8ab46-120">Na koniec należy dodać `AnimationExtender` sterowania do bieżącej strony, w ramach `<form runat="server">` elementu, upewniając się, że animacja jest dołączony i uruchamia:</span><span class="sxs-lookup"><span data-stu-id="8ab46-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="8ab46-121">[![Animacja zostanie utworzony przy użyciu języka C# /VB kod na serwerze](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8ab46-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="8ab46-122">Animacja zostanie utworzony przy użyciu języka C# /VB kod na serwerze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8ab46-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8ab46-123">[Poprzednie](triggering-an-animation-in-another-control-vb.md)
> [dalej](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8ab46-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
