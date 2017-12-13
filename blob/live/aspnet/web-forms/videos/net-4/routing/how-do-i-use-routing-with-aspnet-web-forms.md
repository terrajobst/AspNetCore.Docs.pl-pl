---
uid: web-forms/videos/net-4/routing/how-do-i-use-routing-with-aspnet-web-forms
title: "Jak I: Użyj routingu z formularzami sieci Web ASP.NET? | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "To wideo Pels Krzysztof przedstawia sposób implementowania routingu dla formularzy sieci Web w technologii ASP.NET 4. Po pierwsze pojęcie routingu adres URL jest porównywany mapowanie adresu URL do p..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2010
ms.topic: article
ms.assetid: a3ab6cd9-8f71-4b73-9336-21c0de078269
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-use-routing-with-aspnet-web-forms
msc.type: video
ms.openlocfilehash: f9870dec903ab826e263e9bd7f54ee154da21c48
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-routing-with-aspnet-web-forms"></a><span data-ttu-id="fa9f5-105">Jak I: Użyj routingu z formularzami sieci Web ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="fa9f5-105">How Do I: Use Routing with ASP.NET Web Forms?</span></span>
====================
<span data-ttu-id="fa9f5-106">przez [Pels Krzysztof](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="fa9f5-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="fa9f5-107">To wideo Pels Krzysztof przedstawia sposób implementowania routingu dla formularzy sieci Web w technologii ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="fa9f5-107">In this video Chris Pels shows how to implement routing for Web Forms in ASP.NET 4.</span></span> <span data-ttu-id="fa9f5-108">Po pierwsze pojęcie routingu adres URL jest porównywany mapowanie adresu URL do fizycznego pliku w witrynie.</span><span class="sxs-lookup"><span data-stu-id="fa9f5-108">First, the concept of routing a URL is compared to mapping the URL to a physical file in the site.</span></span> <span data-ttu-id="fa9f5-109">Następnie trasy przykładowy adres URL jest zdefiniowany w pliku global.asax pliku aplikacji\_rozpoczęcia obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="fa9f5-109">Then, a sample route for a URL is defined in the global.asax file Application\_Start event handler.</span></span> <span data-ttu-id="fa9f5-110">Trasy zawiera wartość sparametryzowana, które użytkownik może wprowadzić w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="fa9f5-110">The route contains a parameterized value that the user can enter in the URL.</span></span> <span data-ttu-id="fa9f5-111">Przykładowa strona zostanie utworzona i wartość parametru trasy jest wyodrębniany na stronie\_obciążenia programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="fa9f5-111">A sample page is then created and the route parameter value is extracted in the Page\_Load event handler.</span></span> <span data-ttu-id="fa9f5-112">Następnie drugi trasa jest zdefiniowana, który zawiera wiele parametrów i trasy do tej samej stronie co początkowej trasy.</span><span class="sxs-lookup"><span data-stu-id="fa9f5-112">Next, a second route is defined that has multiple parameters and routes to the same page as the initial route.</span></span> <span data-ttu-id="fa9f5-113">Strona\_obciążenia programu obsługi zdarzeń jest rozwinięta można wyodrębnić wartość parametru dodatkowe trasy i wyświetlać różne informacje w zależności od wartości jakie zostały przekazane do strony.</span><span class="sxs-lookup"><span data-stu-id="fa9f5-113">The Page\_Load event handler is expanded to extract the additional route parameter value and display different information depending upon what values have been passed to the page.</span></span>

[<span data-ttu-id="fa9f5-114">&#9654; Obejrzyj klip wideo (15 minut)</span><span class="sxs-lookup"><span data-stu-id="fa9f5-114">&#9654; Watch video (15 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-routing-with-aspnet-web-forms)

>[!div class="step-by-step"]
<span data-ttu-id="fa9f5-115">[Poprzednie](aspnet-4-quick-hit-outbound-webforms-routing.md)
[dalej](how-do-i-work-with-urls-in-aspnet-routing.md)</span><span class="sxs-lookup"><span data-stu-id="fa9f5-115">[Previous](aspnet-4-quick-hit-outbound-webforms-routing.md)
[Next](how-do-i-work-with-urls-in-aspnet-routing.md)</span></span>
