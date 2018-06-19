---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Cykl życia aplikacji platformy ASP.NET MVC 5 | Dokumentacja firmy Microsoft
author: cephalin
description: Pobierz dokument PDF, który wykresy cyklem życia aplikacji ASP.NET MVC 5. Ten cykl życia dokument przedstawia ogólny widok życia MVC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036495"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="26d20-104">Cykl życia aplikacji platformy ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="26d20-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="26d20-105">przez [Cephas łącz](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="26d20-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="26d20-106">Pobierz dokument PDF</span><span class="sxs-lookup"><span data-stu-id="26d20-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="26d20-107">W tym miejscu możesz pobrać dokument PDF, który wykresy cyklem życia każda aplikacja ASP.NET MVC 5 z otrzymywania HTTP żądania do wysyłania odpowiedzi HTTP z powrotem do klienta.</span><span class="sxs-lookup"><span data-stu-id="26d20-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="26d20-108">Zaprojektowano go jako edukacyjnym narzędzia dla osób, które są nowe w programie ASP.NET MVC, a także jako punkt odniesienia dla tych, którzy muszą przejść do szczegółów w określonych aspektów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="26d20-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="26d20-109">Dokument PDF ma następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="26d20-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="26d20-110">Odpowiednie [obiekcie HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) etapy pomagające zrozumieć, w którym integruje MVC [cyklem życia aplikacji ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).</span><span class="sxs-lookup"><span data-stu-id="26d20-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="26d20-111">Widok wysokiego poziomu cyklem życia aplikacji MVC, gdy znasz etapów głównych, które każda aplikacja MVC przekazuje w potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="26d20-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="26d20-112">Widok szczegółów, w którym przedstawiono ćwiczenia w dół do szczegółów potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="26d20-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="26d20-113">Możesz porównać ogólny widok i widoku szczegółów, aby zobaczyć, jak szczegóły cykle są zbierane w poszczególnych etapów.</span><span class="sxs-lookup"><span data-stu-id="26d20-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="26d20-114">[Pobierz plik PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) wyświetlić większym widoku.</span><span class="sxs-lookup"><span data-stu-id="26d20-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="26d20-115">Położenie i cel wszystkie metody możliwym do zastąpienia w [kontrolera](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) obiektu w potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="26d20-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="26d20-116">Może lub nie ma potrzeby zastąpienia żadnych jedną metodę, ale ważne jest, aby zrozumieć ich roli w cyklu życia aplikacji, dzięki czemu można napisać kod na etapie cyklu życia odpowiednie dla efektu, które mają.</span><span class="sxs-lookup"><span data-stu-id="26d20-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="26d20-117">Diagramy skierowany w górę przedstawiający sposób wywoływania każdego typu filtru (uwierzytelniania, autoryzacji, działania i wyników).</span><span class="sxs-lookup"><span data-stu-id="26d20-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="26d20-118">Połączyć artykuł lub blogu z każdego punktu odsetek w widoku szczegółów.</span><span class="sxs-lookup"><span data-stu-id="26d20-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="26d20-119">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="26d20-119">Next Steps</span></span>

<span data-ttu-id="26d20-120">Ten dokument spełnia Twoje potrzeby?</span><span class="sxs-lookup"><span data-stu-id="26d20-120">Does this document meet your need?</span></span> <span data-ttu-id="26d20-121">Prosimy o wyrażenie opinii.</span><span class="sxs-lookup"><span data-stu-id="26d20-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="26d20-122">Jeśli masz pytania na cyklu życia ASP.NET MVC w aplikacji, [Stackoverflow](http://stackoverflow.com/help) i [ASP.NET MVC forum](https://forums.asp.net/1146.aspx) są doskonałe miejsca, aby zadać.</span><span class="sxs-lookup"><span data-stu-id="26d20-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="26d20-123">Postępuj zgodnie z [mnie](https://twitter.com/Cephas_MSFT) w serwisie twitter, aby aktualizacje na Moje najnowsze samouczki.</span><span class="sxs-lookup"><span data-stu-id="26d20-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
