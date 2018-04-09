---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modyfikowanie animacji po stronie serwera (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Może to również animacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b9ce85fc5040b2318233b3c553c2cf53dd03555
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="9d705-104">Modyfikowanie animacji po stronie serwera (VB)</span><span class="sxs-lookup"><span data-stu-id="9d705-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="9d705-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9d705-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9d705-106">[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9d705-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="9d705-107">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="9d705-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9d705-108">Animacji mogą również zostać zmienione po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="9d705-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="9d705-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="9d705-109">Overview</span></span>

<span data-ttu-id="9d705-110">Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu.</span><span class="sxs-lookup"><span data-stu-id="9d705-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9d705-111">Animacji mogą również zostać zmienione po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="9d705-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="9d705-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="9d705-112">Steps</span></span>

<span data-ttu-id="9d705-113">Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="9d705-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="9d705-114">Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="9d705-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="9d705-115">Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:</span><span class="sxs-lookup"><span data-stu-id="9d705-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="9d705-116">Pozostała część kodu jest uruchamiana po stronie serwera i nie używa znaczników; Zamiast tego kod używane w celu utworzenia `AnimationExtender` sterowania:</span><span class="sxs-lookup"><span data-stu-id="9d705-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="9d705-117">Jednak zestaw narzędzi do sterowania aktualnie nie zapewnia dostęp API do tworzenia indywidualnych animacji.</span><span class="sxs-lookup"><span data-stu-id="9d705-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="9d705-118">Jest jednak można ustawić `AnimationExtender`właściwości animacji na ciąg zawierający znaczników XML używane podczas przypisywania animacji deklaratywnie.</span><span class="sxs-lookup"><span data-stu-id="9d705-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="9d705-119">Aby utworzyć plik XML, który nie może zawierać `<Animations>` elementu można użyć programu .NET Framework XML obsługuje lub zgodnie z poniższym kodem, wystarczy podać ciąg:</span><span class="sxs-lookup"><span data-stu-id="9d705-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="9d705-120">Na koniec należy dodać `AnimationExtender` sterowania do bieżącej strony w `<form runat="server">` elementu, upewniając się, czy animacja jest dołączony i uruchamia:</span><span class="sxs-lookup"><span data-stu-id="9d705-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="9d705-121">[![Animacja jest tworzony przy użyciu po stronie serwera C# /VB kodu](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9d705-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="9d705-122">Animacja jest tworzony przy użyciu po stronie serwera C# / VB. kodu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9d705-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9d705-123">[Poprzednie](triggering-an-animation-in-another-control-vb.md)
> [dalej](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9d705-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
