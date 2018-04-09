---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Tworzenie trasy niestandardowe (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać trasy niestandardowe do aplikacji platformy ASP.NET MVC. W tym samouczku Dowiedz się jak zmodyfikować domyślną tabelę routingu w pliku Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: e827725ab7ce54c86ae9f4193d0a8a7ef4af8512
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-routes-vb"></a><span data-ttu-id="67183-104">Tworzenie trasy niestandardowe (VB)</span><span class="sxs-lookup"><span data-stu-id="67183-104">Creating Custom Routes (VB)</span></span>
====================
<span data-ttu-id="67183-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="67183-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="67183-106">Dowiedz się, jak dodać trasy niestandardowe do aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="67183-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="67183-107">W tym samouczku Dowiedz się jak zmodyfikować domyślną tabelę routingu w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="67183-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="67183-108">W tym samouczku Dowiedz się jak dodać niestandardowe trasy do aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="67183-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="67183-109">Jak zmodyfikować tabeli trasy domyślnej w pliku Global.asax z tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="67183-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="67183-110">W aplikacjach ASP.NET MVC domyślne tabeli tras będzie działać bez problemu.</span><span class="sxs-lookup"><span data-stu-id="67183-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="67183-111">Jednak może się okazać, że ma specjalizowany potrzeb routingu.</span><span class="sxs-lookup"><span data-stu-id="67183-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="67183-112">W takim przypadku można utworzyć tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="67183-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="67183-113">Załóżmy, na przykład tworzysz aplikację blogu.</span><span class="sxs-lookup"><span data-stu-id="67183-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="67183-114">Można obsłużyć żądań przychodzących, które wyglądają następująco:</span><span class="sxs-lookup"><span data-stu-id="67183-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="67183-115">/ Archiwum 12-25-2009</span><span class="sxs-lookup"><span data-stu-id="67183-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="67183-116">Gdy użytkownik wprowadza to żądanie, chcesz przywrócić wpis w blogu, która odpowiada Data 2009-12/25.</span><span class="sxs-lookup"><span data-stu-id="67183-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="67183-117">Aby obsługiwać ten typ żądania, należy utworzyć niestandardowe trasy.</span><span class="sxs-lookup"><span data-stu-id="67183-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="67183-118">Plik Global.asax 1 Lista zawiera nowe niestandardowe trasy, o nazwie blogu w żądaniach dojść, które mają postać /Archive/*Data wprowadzenia*.</span><span class="sxs-lookup"><span data-stu-id="67183-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="67183-119">**Wyświetlanie listy 1 - Global.asax (z tras niestandardowych)**</span><span class="sxs-lookup"><span data-stu-id="67183-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="67183-120">Kolejność trasy, które można dodać do tabeli tras jest ważna.</span><span class="sxs-lookup"><span data-stu-id="67183-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="67183-121">Nasze nowe niestandardowe trasy blogu zostanie dodany przed istniejącą trasę domyślną.</span><span class="sxs-lookup"><span data-stu-id="67183-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="67183-122">Jeśli odwrócić kolejność, następnie trasy domyślnej zawsze będzie uzyskać nazwę zamiast tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="67183-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="67183-123">Niestandardowe trasy blogu odpowiada każde żądanie, która rozpoczyna się od/archiwum /.</span><span class="sxs-lookup"><span data-stu-id="67183-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="67183-124">Tak jest on zgodny wszystkich następujących adresów URL:</span><span class="sxs-lookup"><span data-stu-id="67183-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="67183-125">/ Archiwum 12-25-2009</span><span class="sxs-lookup"><span data-stu-id="67183-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="67183-126">-Archiwum 10-6-2004</span><span class="sxs-lookup"><span data-stu-id="67183-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="67183-127">/ Archiwum/firmy apple</span><span class="sxs-lookup"><span data-stu-id="67183-127">/Archive/apple</span></span>

<span data-ttu-id="67183-128">Tras niestandardowych mapuje przychodzące żądanie do kontrolera o nazwie archiwum i wywołuje akcję Entry().</span><span class="sxs-lookup"><span data-stu-id="67183-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="67183-129">Gdy wywoływana jest metoda Entry(), daty rozpoczęcia jest przekazywana jako parametr o nazwie entryDate.</span><span class="sxs-lookup"><span data-stu-id="67183-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="67183-130">Można użyć tras niestandardowych blogu kontroler w wyświetlania 2.</span><span class="sxs-lookup"><span data-stu-id="67183-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="67183-131">**Wyświetlanie listy 2 - ArchiveController.vb**</span><span class="sxs-lookup"><span data-stu-id="67183-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="67183-132">Zwróć uwagę, że metoda Entry() wyświetlania 2 przyjmuje parametr typu DateTime.</span><span class="sxs-lookup"><span data-stu-id="67183-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="67183-133">Struktura MVC jest inteligentne przekonwertować datę wejścia z adresu URL na wartość daty i godziny automatycznie.</span><span class="sxs-lookup"><span data-stu-id="67183-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="67183-134">Jeśli parametr Data wpisu z adresu URL nie można przekonwertować na wartość typu DateTime, występuje błąd (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="67183-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="67183-135">**Rysunek 1 — błąd podczas konwersji parametru**</span><span class="sxs-lookup"><span data-stu-id="67183-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="67183-136">[![Okno dialogowe nowego projektu](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="67183-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="67183-137">**Rysunek 01**: błąd podczas konwersji parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-routes-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="67183-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="67183-138">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="67183-138">Summary</span></span>

<span data-ttu-id="67183-139">Celem tego samouczka było wykazać, sposób tworzenia tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="67183-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="67183-140">Przedstawiono sposób dodawania tras niestandardowych do tabeli tras w pliku Global.asax, który reprezentuje wpisy.</span><span class="sxs-lookup"><span data-stu-id="67183-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="67183-141">Omówiono sposób mapowania żądania do wpisów w kontrolerze o nazwie ArchiveController i o nazwie Entry() akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="67183-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="67183-142">[Poprzednie](asp-net-mvc-controller-overview-vb.md)
> [dalej](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="67183-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
