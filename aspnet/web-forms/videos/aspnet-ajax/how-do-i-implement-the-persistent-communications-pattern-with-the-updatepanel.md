---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel
title: "[Jak i.] Implementowanie wzorca trwałe komunikacji z UpdatePanel? | Dokumentacja firmy Microsoft"
author: JoeStagner
description: "W witrynie sieci Web tradycyjnych przeglądarkę i serwer nie obsługują stałą łączność, ale komunikować się tylko w odpowiedzi na jakiejś czynności użytkownika..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/01/2007
ms.topic: article
ms.assetid: 49c7a74d-dce7-4d5c-8282-c7846f478e11
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel
msc.type: video
ms.openlocfilehash: 97b653d024c4f109014e2b98025274fa51c15827
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel"></a><span data-ttu-id="00882-104">[Jak i.] Implementowanie wzorca trwałe komunikacji z UpdatePanel?</span><span class="sxs-lookup"><span data-stu-id="00882-104">[How Do I:] Implement the Persistent Communications Pattern with the UpdatePanel?</span></span>
====================
<span data-ttu-id="00882-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="00882-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="00882-106">W witrynie sieci Web tradycyjnych przeglądarkę i serwer nie obsługują stałą łączność, ale komunikować się tylko w odpowiedzi na użytkownika wykonującego akcję.</span><span class="sxs-lookup"><span data-stu-id="00882-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="00882-107">W nowoczesnych witryny sieci Web, gdy strona staje się kontenerem aplikacji może być korzystne dla przeglądarki i serwera do obsługi komunikacji trwającą tak, aby strona aktualizacji może wystąpić bez wykonywania akcji przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="00882-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="00882-108">Jest to nazywane stałe wzorzec komunikacji dla technologii AJAX.</span><span class="sxs-lookup"><span data-stu-id="00882-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="00882-109">ASP.NET AJAX udostępnia dwa sposoby głównego dla deweloperów sieci Web implementacji klienta wzorca trwałe komunikacji.</span><span class="sxs-lookup"><span data-stu-id="00882-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="00882-110">To wideo pokazuje prosty sposób jest użycie ASP.NET AJAX UpdatePanel jako podstawa operacji wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="00882-110">This video demonstrates the simple way, which is to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="00882-111">Później wideo firma Microsoft będzie sposób implementacji tego samego wzorca bez użycia elementu ASP.NET AJAX UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="00882-111">In a later video we will learn how to implement the same pattern without the use of the ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="00882-112">&#9654; Obejrzyj klip wideo (minuty 12)</span><span class="sxs-lookup"><span data-stu-id="00882-112">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel)

>[!div class="step-by-step"]
<span data-ttu-id="00882-113">[Poprzednie](how-do-i-use-the-conditional-updatemode-of-the-updatepanel.md)
[dalej](how-do-i-localize-an-aspnet-ajax-application.md)</span><span class="sxs-lookup"><span data-stu-id="00882-113">[Previous](how-do-i-use-the-conditional-updatemode-of-the-updatepanel.md)
[Next](how-do-i-localize-an-aspnet-ajax-application.md)</span></span>
