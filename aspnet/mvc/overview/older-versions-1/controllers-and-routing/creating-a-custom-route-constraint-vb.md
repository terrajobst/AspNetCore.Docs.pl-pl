---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Utworzenie ograniczenia trasy niestandardowe (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: Stephen Walther przedstawiono sposób tworzenia tras niestandardowych ograniczenia. Możemy wdrożyć prosty ograniczenie niestandardowych, które zapobiega trasę dopasowywane w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 094077fa0cb546f4cc91dbf074f8014e62b3b19c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="4d515-104">Utworzenie ograniczenia trasy niestandardowe (VB)</span><span class="sxs-lookup"><span data-stu-id="4d515-104">Creating a Custom Route Constraint (VB)</span></span>
====================
<span data-ttu-id="4d515-105">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="4d515-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="4d515-106">Stephen Walther przedstawiono sposób tworzenia tras niestandardowych ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="4d515-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="4d515-107">Wdrożymy proste ograniczenie niestandardowych, które zapobiega trasę filtrowanego, gdy żądanie przeglądarki na komputerze zdalnym.</span><span class="sxs-lookup"><span data-stu-id="4d515-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="4d515-108">Celem tego samouczka jest aby zademonstrować, jak można utworzyć ograniczenia tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="4d515-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="4d515-109">Ograniczenia trasy niestandardowe umożliwia uniemożliwić filtrowanego, chyba że niektóre warunek niestandardowy jest dopasowywany trasy.</span><span class="sxs-lookup"><span data-stu-id="4d515-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="4d515-110">W tym samouczku utworzymy ograniczenia trasy Localhost.</span><span class="sxs-lookup"><span data-stu-id="4d515-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="4d515-111">Ograniczenia trasy Localhost zgodny tylko żądań z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="4d515-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="4d515-112">Żądania zdalne z Internetu są niezgodne.</span><span class="sxs-lookup"><span data-stu-id="4d515-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="4d515-113">Ograniczenia trasy niestandardowe można wdrożyć przy implementującej interfejs interfejs IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="4d515-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="4d515-114">Jest to bardzo proste interfejsu, który opisuje metodę pojedynczego:</span><span class="sxs-lookup"><span data-stu-id="4d515-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="4d515-115">Metoda zwraca wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="4d515-115">The method returns a Boolean value.</span></span> <span data-ttu-id="4d515-116">Jeśli zwróci wartość False, skojarzonych z ograniczeniem trasy nie pasuje do żądania przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4d515-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="4d515-117">Ograniczenie Localhost znajduje się w 1 wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="4d515-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="4d515-118">**Listing 1 - LocalhostConstraint.vb**</span><span class="sxs-lookup"><span data-stu-id="4d515-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="4d515-119">Ograniczenia w 1 wyświetlania korzysta z właściwości IsLocal udostępniane przez klasę HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="4d515-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="4d515-120">Ta właściwość zwraca wartość true, gdy adres IP żądania jest albo 127.0.0.1 lub adres IP żądania jest taki sam jak adres IP serwera.</span><span class="sxs-lookup"><span data-stu-id="4d515-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="4d515-121">Możesz użyć niestandardowego ograniczenia w obrębie trasy zdefiniowane w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="4d515-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="4d515-122">Plik Global.asax wyświetlania 2 używa ograniczenia Localhost aby uniemożliwić osobom żądania strony administratora, chyba że finalizowania żądania z lokalnego serwera.</span><span class="sxs-lookup"><span data-stu-id="4d515-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="4d515-123">Na przykład żądania /Admin/DeleteAll zakończy się niepowodzeniem, gdy na serwerze zdalnym.</span><span class="sxs-lookup"><span data-stu-id="4d515-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="4d515-124">**Wyświetlanie listy 2 - pliku Global.asax**</span><span class="sxs-lookup"><span data-stu-id="4d515-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="4d515-125">Warunek ograniczający Localhost jest używany w definicji trasy administratora.</span><span class="sxs-lookup"><span data-stu-id="4d515-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="4d515-126">Nie można dopasować tej trasy przez żądanie przeglądarki zdalnego.</span><span class="sxs-lookup"><span data-stu-id="4d515-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="4d515-127">Należy pamiętać, jednak, że innych tras zdefiniowanych w pliku Global.asax może pasuje do tego samego żądania.</span><span class="sxs-lookup"><span data-stu-id="4d515-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="4d515-128">Należy zrozumieć ograniczenie uniemożliwia określonej trasy dopasowywanie na żądanie, a nie wszystkich tras określonych w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="4d515-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="4d515-129">Zwróć uwagę, że trasy domyślnej zostały oznaczone komentarzami z pliku Global.asax wyświetlania 2.</span><span class="sxs-lookup"><span data-stu-id="4d515-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="4d515-130">Jeśli dołączysz trasa domyślna trasa domyślna umożliwi dopasowanie żądania dla administratora kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4d515-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="4d515-131">W takim przypadku użytkownicy zdalni nadal może wywołać akcji kontrolera administratora, nawet jeśli ich żądania nie pasuje trasy administratora.</span><span class="sxs-lookup"><span data-stu-id="4d515-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4d515-132">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="4d515-132">Previous</span></span>](creating-a-route-constraint-vb.md)
