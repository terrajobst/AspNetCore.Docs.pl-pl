---
uid: web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
title: 'Jak I: pracy z adresami URL w proces routingu platformy ASP.NET? | Dokumentacja firmy Microsoft'
author: rick-anderson
description: "W tym wideo Pels Krzysztof pokazuje, jak określać adresy URL w witrynie sieci web, korzysta z routingu platformy ASP.NET. Po pierwsze witryna sieci web została utworzona i routing jest zdefiniowany w k/g..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2010
ms.topic: article
ms.assetid: 08f9d0a7-cfa0-4914-a672-8a64295d7ba8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
msc.type: video
ms.openlocfilehash: cf79019d52bbf34bc604ab6289fcc3c48533f40c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-work-with-urls-in-aspnet-routing"></a><span data-ttu-id="d1d66-105">Jak I: pracy z adresami URL w proces routingu platformy ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="d1d66-105">How Do I: Work with URLs in ASP.NET Routing?</span></span>
====================
<span data-ttu-id="d1d66-106">przez [Pels Krzysztof](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="d1d66-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="d1d66-107">W tym wideo Pels Krzysztof pokazuje, jak określać adresy URL w witrynie sieci web, korzysta z routingu platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d1d66-107">In this video Chris Pels shows how to specify URLs in a web site that utilizes ASP.NET routing.</span></span> <span data-ttu-id="d1d66-108">Najpierw witryny sieci web jest tworzona i routing jest zdefiniowany w globalnej klasy aplikacji (.asax).</span><span class="sxs-lookup"><span data-stu-id="d1d66-108">First, a web site is created and routing is defined in the Global Application Class (.asax).</span></span> <span data-ttu-id="d1d66-109">Następnie przykładowa strona sieci web jest tworzona i adres URL oparty na zdefiniowanej trasie jest dodany do strony przy użyciu standardowego "twarde kodowany" podejście, np. "~/Stats/Visitors".</span><span class="sxs-lookup"><span data-stu-id="d1d66-109">Next, a sample web page is created and a URL based upon a defined route is added to the page using the standard "hard coded" approach, e.g., "~/Stats/Visitors".</span></span> <span data-ttu-id="d1d66-110">Inne łącze jest dodawane do strony, który dynamicznie generuje ten sam adres URL w znaczniku przy użyciu metody RouteValue, która przyjmuje nazwy trasy i parametry.</span><span class="sxs-lookup"><span data-stu-id="d1d66-110">Another link is then added to the page which dynamically generates the same URL in markup using the RouteValue method which accepts the route name and parameters.</span></span> <span data-ttu-id="d1d66-111">Ten sam adres URL jest następnie zaimplementowana przy użyciu kodu, a nie znaczników bezpośrednio na stronie.</span><span class="sxs-lookup"><span data-stu-id="d1d66-111">The same URL is then implemented using code rather than markup directly in the page.</span></span> <span data-ttu-id="d1d66-112">Następnie zmienić oryginalnego trasy i strony fizycznej lokalizacji, powodujące zakodowany łącze nie jest już pracy, natomiast zarówno dynamicznie generowanego łącza funkcji poprawnie.</span><span class="sxs-lookup"><span data-stu-id="d1d66-112">The original route and physical page location are then changed, resulting in the hard coded link no longer working whereas both dynamically generated links function properly.</span></span> <span data-ttu-id="d1d66-113">Na koniec następnie opisanej wartość dynamicznie generowanym łącza.</span><span class="sxs-lookup"><span data-stu-id="d1d66-113">Finally, the value of dynamically generated links is then discussed.</span></span>

[<span data-ttu-id="d1d66-114">&#9654; Obejrzyj klip wideo (20 minut)</span><span class="sxs-lookup"><span data-stu-id="d1d66-114">&#9654; Watch video (20 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-work-with-urls-in-aspnet-routing)

>[!div class="step-by-step"]
[<span data-ttu-id="d1d66-115">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="d1d66-115">Previous</span></span>](how-do-i-use-routing-with-aspnet-web-forms.md)
