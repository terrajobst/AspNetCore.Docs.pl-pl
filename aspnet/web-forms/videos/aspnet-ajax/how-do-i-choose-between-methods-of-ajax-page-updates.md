---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Jak mogę] Wybieranie spośród metod AJAX aktualizacji stron? | Microsoft Docs'
author: JoeStagner
description: W tym filmie pokazano Stagner Jan porównanie dwóch głównych metodach wykonywania aktualizacji stron stylu AJAX w aplikacji ASP.NET. Pierwsza metoda jest używać Upd...
ms.author: aspnetcontent
ms.date: 07/09/2007
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: 226fb0423ea05ad9034c909037358331918f2892
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838442"
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="d582f-105">[Jak mogę] Wybieranie spośród metod AJAX aktualizacji stron?</span><span class="sxs-lookup"><span data-stu-id="d582f-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="d582f-106">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="d582f-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="d582f-107">W tym filmie pokazano Stagner Jan porównanie dwóch głównych metodach wykonywania aktualizacji stron stylu AJAX w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d582f-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="d582f-108">Pierwsza metoda jest używać kontrolki UpdatePanel z których żaden dodatkowy kod nie musi być napisany na po stronie klienta lub po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="d582f-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="d582f-109">Zaletą używania kontrolki UpdatePanel to, że wszystko działa automatycznie.</span><span class="sxs-lookup"><span data-stu-id="d582f-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="d582f-110">Kary jest to, że po stronie klienta wymaga dużej ilości danych, które mają zostać uwzględnione w technologii AJAX żądania i odpowiedzi, a na serwerze, wymaga on cykl życia pełnej strony do wykonania.</span><span class="sxs-lookup"><span data-stu-id="d582f-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="d582f-111">Druga metoda jest używać wywołań zwrotnych sieci, w którym dodatkowy kod musi być napisany zarówno po stronie klienta, jak i po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="d582f-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="d582f-112">Zaletą używania wywołań zwrotnych sieci jest, że u klienta wymaga bardzo małą ilością danych, które mają zostać uwzględnione w technologii AJAX żądania i odpowiedzi i na serwerze wymaga tylko metody o nazwie usługi do wykonania.</span><span class="sxs-lookup"><span data-stu-id="d582f-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="d582f-113">Penality jest czas i nakład pracy potrzebny do pisania kodu, niezbędne.</span><span class="sxs-lookup"><span data-stu-id="d582f-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="d582f-114">Jan kończy wideo od Omawiając, jakie należy rozważyć podczas wybierania pomiędzy dwóch głównych metodach aktualizacji stron AJAX stylu.</span><span class="sxs-lookup"><span data-stu-id="d582f-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="d582f-115">(W tym wideo używa kodu z [jak rozpocząć pracę z ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) wideo i [jak mogę utworzyć po stronie klienta wywołań zwrotnych sieci przy użyciu rozszerzeń ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) wideo.)</span><span class="sxs-lookup"><span data-stu-id="d582f-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="d582f-116">&#9654;Obejrzyj film wideo (11 minut)</span><span class="sxs-lookup"><span data-stu-id="d582f-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="d582f-117">[Poprzednie](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [dalej](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="d582f-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
