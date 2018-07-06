---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Tworzenie niestandardowego ograniczenia trasy (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Autor: Stephen Walther pokazuje, jak można utworzyć ograniczenia trasy niestandardowej. Wdrożymy prosty niestandardowe ograniczenia, które uniemożliwia trasę dopasowywane w...'
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: e21e7e027cf66f390fc37ec08a07ae007e8242c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831816"
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="47b71-104">Tworzenie niestandardowego ograniczenia trasy (C#)</span><span class="sxs-lookup"><span data-stu-id="47b71-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="47b71-105">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="47b71-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="47b71-106">Autor: Stephen Walther pokazuje, jak można utworzyć ograniczenia trasy niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="47b71-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="47b71-107">Firma Microsoft zaimplementowania proste ograniczenie niestandardowych, które uniemożliwia trasy są dopasowywane, gdy żądanie przeglądarki na komputerze zdalnym.</span><span class="sxs-lookup"><span data-stu-id="47b71-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="47b71-108">Celem tego samouczka jest pokazują, jak można utworzyć ograniczenia trasy niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="47b71-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="47b71-109">Ograniczenia trasy niestandardowej można uniemożliwić trasy są dopasowywane, chyba że jakiś warunek niestandardowy jest zgodny.</span><span class="sxs-lookup"><span data-stu-id="47b71-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="47b71-110">W tym samouczku utworzymy Localhost ograniczenia trasy.</span><span class="sxs-lookup"><span data-stu-id="47b71-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="47b71-111">Ograniczenia trasy Localhost jest zgodny tylko żądań wysyłanych z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="47b71-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="47b71-112">Żądania zdalne z przez Internet nie zostały dopasowane.</span><span class="sxs-lookup"><span data-stu-id="47b71-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="47b71-113">Implementując interfejs IRouteConstraint implementowania ograniczenia trasy niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="47b71-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="47b71-114">Jest to bardzo prosty interfejs, który opisuje pojedynczej metody:</span><span class="sxs-lookup"><span data-stu-id="47b71-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="47b71-115">Metoda zwraca wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="47b71-115">The method returns a Boolean value.</span></span> <span data-ttu-id="47b71-116">Po powrocie do false trasy skojarzonych z ograniczeniem nie będzie zgodne żądanie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="47b71-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="47b71-117">Ograniczenie Localhost znajduje się w ofercie 1.</span><span class="sxs-lookup"><span data-stu-id="47b71-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="47b71-118">**Wyświetlanie listy 1 - LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="47b71-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="47b71-119">Ograniczenia w ofercie 1 korzysta z właściwości IsLocal udostępnianych przez klasy HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="47b71-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="47b71-120">Ta właściwość zwraca wartość true, gdy adres IP żądania jest albo 127.0.0.1 lub adres IP żądania jest taka sama jak adres IP serwera.</span><span class="sxs-lookup"><span data-stu-id="47b71-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="47b71-121">Możesz użyć niestandardowego ograniczenia w obrębie trasy zdefiniowanej w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="47b71-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="47b71-122">Plik Global.asax w ofercie 2 używa ograniczenie Localhost, aby uniemożliwić osobom żądania na stronie administratora, chyba że dokonają żądania z serwera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="47b71-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="47b71-123">Na przykład żądanie /Admin/DeleteAll zakończy się niepowodzeniem, gdy na serwerze zdalnym.</span><span class="sxs-lookup"><span data-stu-id="47b71-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="47b71-124">**Wyświetlanie listy 2 - w pliku Global.asax**</span><span class="sxs-lookup"><span data-stu-id="47b71-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="47b71-125">Ograniczenie Localhost jest używane w definicji trasy administratora.</span><span class="sxs-lookup"><span data-stu-id="47b71-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="47b71-126">Ta trasa nie będzie można dopasować do podwyrażenia żądania zdalnego przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="47b71-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="47b71-127">Należy pamiętać, jednak czy innych tras zdefiniowanych w pliku Global.asax może pasuje do tego samego żądania.</span><span class="sxs-lookup"><span data-stu-id="47b71-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="47b71-128">Jest ważne dowiedzieć się, że ograniczenie zapobiega określonej trasy pasujące żądanie i nie wszystkie trasy zdefiniowanej w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="47b71-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="47b71-129">Należy zauważyć, że domyślna trasa zostały oznaczone komentarzami w pliku Global.asax w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="47b71-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="47b71-130">Jeśli uwzględniony trasa domyślna trasa domyślna umożliwi dopasowanie żądania dla kontrolera administratora.</span><span class="sxs-lookup"><span data-stu-id="47b71-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="47b71-131">W takim przypadku użytkownicy zdalni nadal może wywołać akcji kontrolera administratora, nawet jeśli ich żądania nie odpowiada trasy administratora.</span><span class="sxs-lookup"><span data-stu-id="47b71-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="47b71-132">[Poprzednie](creating-a-route-constraint-cs.md)
> [dalej](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="47b71-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
