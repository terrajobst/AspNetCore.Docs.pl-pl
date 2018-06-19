---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Utworzenie ograniczenia trasy niestandardowe (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: Stephen Walther przedstawiono sposób tworzenia tras niestandardowych ograniczenia. Możemy wdrożyć prosty ograniczenie niestandardowych, które zapobiega trasę dopasowywane w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c120a102b117433b6774f2ea7800f1c4a609f8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874346"
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="60b8d-104">Utworzenie ograniczenia trasy niestandardowe (C#)</span><span class="sxs-lookup"><span data-stu-id="60b8d-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="60b8d-105">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="60b8d-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="60b8d-106">Stephen Walther przedstawiono sposób tworzenia tras niestandardowych ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="60b8d-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="60b8d-107">Wdrożymy proste ograniczenie niestandardowych, które zapobiega trasę filtrowanego, gdy żądanie przeglądarki na komputerze zdalnym.</span><span class="sxs-lookup"><span data-stu-id="60b8d-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="60b8d-108">Celem tego samouczka jest aby zademonstrować, jak można utworzyć ograniczenia tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="60b8d-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="60b8d-109">Ograniczenia trasy niestandardowe umożliwia uniemożliwić filtrowanego, chyba że niektóre warunek niestandardowy jest dopasowywany trasy.</span><span class="sxs-lookup"><span data-stu-id="60b8d-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="60b8d-110">W tym samouczku utworzymy ograniczenia trasy Localhost.</span><span class="sxs-lookup"><span data-stu-id="60b8d-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="60b8d-111">Ograniczenia trasy Localhost zgodny tylko żądań z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="60b8d-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="60b8d-112">Żądania zdalne z Internetu są niezgodne.</span><span class="sxs-lookup"><span data-stu-id="60b8d-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="60b8d-113">Ograniczenia trasy niestandardowe można wdrożyć przy implementującej interfejs interfejs IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="60b8d-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="60b8d-114">Jest to bardzo proste interfejsu, który opisuje metodę pojedynczego:</span><span class="sxs-lookup"><span data-stu-id="60b8d-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="60b8d-115">Metoda zwraca wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="60b8d-115">The method returns a Boolean value.</span></span> <span data-ttu-id="60b8d-116">Jeśli wartość false, skojarzonych z ograniczeniem trasy nie pasuje do żądania przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="60b8d-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="60b8d-117">Ograniczenie Localhost znajduje się w 1 wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="60b8d-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="60b8d-118">**Wyświetlanie listy 1 - LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="60b8d-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="60b8d-119">Ograniczenia w 1 wyświetlania korzysta z właściwości IsLocal udostępniane przez klasę HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="60b8d-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="60b8d-120">Ta właściwość zwraca wartość true, gdy adres IP żądania jest albo 127.0.0.1 lub adres IP żądania jest taki sam jak adres IP serwera.</span><span class="sxs-lookup"><span data-stu-id="60b8d-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="60b8d-121">Możesz użyć niestandardowego ograniczenia w obrębie trasy zdefiniowane w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="60b8d-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="60b8d-122">Plik Global.asax wyświetlania 2 używa ograniczenia Localhost aby uniemożliwić osobom żądania strony administratora, chyba że finalizowania żądania z lokalnego serwera.</span><span class="sxs-lookup"><span data-stu-id="60b8d-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="60b8d-123">Na przykład żądania /Admin/DeleteAll zakończy się niepowodzeniem, gdy na serwerze zdalnym.</span><span class="sxs-lookup"><span data-stu-id="60b8d-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="60b8d-124">**Wyświetlanie listy 2 - pliku Global.asax**</span><span class="sxs-lookup"><span data-stu-id="60b8d-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="60b8d-125">Warunek ograniczający Localhost jest używany w definicji trasy administratora.</span><span class="sxs-lookup"><span data-stu-id="60b8d-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="60b8d-126">Nie można dopasować tej trasy przez żądanie przeglądarki zdalnego.</span><span class="sxs-lookup"><span data-stu-id="60b8d-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="60b8d-127">Należy pamiętać, jednak, że innych tras zdefiniowanych w pliku Global.asax może pasuje do tego samego żądania.</span><span class="sxs-lookup"><span data-stu-id="60b8d-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="60b8d-128">Należy zrozumieć ograniczenie uniemożliwia określonej trasy dopasowywanie na żądanie, a nie wszystkich tras określonych w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="60b8d-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="60b8d-129">Zwróć uwagę, że trasy domyślnej zostały oznaczone komentarzami z pliku Global.asax wyświetlania 2.</span><span class="sxs-lookup"><span data-stu-id="60b8d-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="60b8d-130">Jeśli dołączysz trasa domyślna trasa domyślna umożliwi dopasowanie żądania dla administratora kontrolera.</span><span class="sxs-lookup"><span data-stu-id="60b8d-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="60b8d-131">W takim przypadku użytkownicy zdalni nadal może wywołać akcji kontrolera administratora, nawet jeśli ich żądania nie pasuje trasy administratora.</span><span class="sxs-lookup"><span data-stu-id="60b8d-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="60b8d-132">[Poprzednie](creating-a-route-constraint-cs.md)
> [dalej](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="60b8d-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
