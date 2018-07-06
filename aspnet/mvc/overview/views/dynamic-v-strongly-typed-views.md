---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamiczne v. Silnie Typizowane widoki | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 941cb24b81721eb75a8f7150ddb17acf71287da3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822185"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="3a9bd-103">Dynamiczne v.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-103">Dynamic v.</span></span> <span data-ttu-id="3a9bd-104">Silnie Typizowane widoki</span><span class="sxs-lookup"><span data-stu-id="3a9bd-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="3a9bd-105">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="3a9bd-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="3a9bd-106">Istnieją trzy sposoby przekazywania informacji z kontrolera do widoku w ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="3a9bd-107">Jako obiekt silnie typizowany model.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="3a9bd-108">Jako typu dynamicznego (przy użyciu @model dynamiczny)</span><span class="sxs-lookup"><span data-stu-id="3a9bd-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="3a9bd-109">Za pomocą obiekt ViewBag</span><span class="sxs-lookup"><span data-stu-id="3a9bd-109">Using the ViewBag</span></span>

<span data-ttu-id="3a9bd-110">Moje recenzje prostą aplikację MVC 3 pierwszych blogu do Porównaj i zestaw widoki dynamiczne i silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="3a9bd-111">Kontroler rozpoczyna się od prostego listę blogów:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="3a9bd-112">Kliknij prawym przyciskiem myszy w metodzie IndexNotStonglyTyped() i Dodawanie widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="3a9bd-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3a9bd-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="3a9bd-114">Upewnij się, że **utworzyć widok silnie typizowane** pole nie jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="3a9bd-115">Wynikowym widoku nie zawiera wiele:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="3a9bd-116">Ponieważ używamy dynamiczne i silnie typizowanego widoku, funkcja intellisense nie pomagają nam.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="3a9bd-117">Kompletny kod jest pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="3a9bd-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="3a9bd-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="3a9bd-119">Teraz dodamy silnie typizowanego widoku.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="3a9bd-120">Dodaj następujący kod na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="3a9bd-121">Zwróć uwagę, że jest dokładnie ten sam zwracany View(topBlogs); Wywołaj jako — silnie typizowanego widoku.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="3a9bd-122">Kliknij prawym przyciskiem myszy wewnątrz *StonglyTypedIndex()* i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="3a9bd-123">Tym razem wybierz pozycję **Blog** klasa modelu, a następnie wybierz pozycję **listy** jako szablon szkieletu.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="3a9bd-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="3a9bd-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="3a9bd-125">Wewnątrz szablonu widoku uzyskujemy obsługę funkcji intellisense.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="3a9bd-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="3a9bd-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="3a9bd-127">Projekt c# można pobrać [tutaj](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="3a9bd-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
