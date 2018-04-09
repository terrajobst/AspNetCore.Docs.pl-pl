---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Tworzenie trasy niestandardowe (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać trasy niestandardowe do aplikacji platformy ASP.NET MVC. W tym samouczku Dowiedz się jak zmodyfikować domyślną tabelę routingu w pliku Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 573b6a3360124feea92788ff7a3de363840fa1ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-routes-c"></a><span data-ttu-id="00107-104">Tworzenie trasy niestandardowe (C#)</span><span class="sxs-lookup"><span data-stu-id="00107-104">Creating Custom Routes (C#)</span></span>
====================
<span data-ttu-id="00107-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="00107-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="00107-106">Dowiedz się, jak dodać trasy niestandardowe do aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00107-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="00107-107">W tym samouczku Dowiedz się jak zmodyfikować domyślną tabelę routingu w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="00107-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="00107-108">W tym samouczku Dowiedz się jak dodać niestandardowe trasy do aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00107-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="00107-109">Jak zmodyfikować tabeli trasy domyślnej w pliku Global.asax z tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="00107-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="00107-110">Dla wielu aplikacji ASP.NET MVC proste tabela tras domyślnych będzie działać bez problemu.</span><span class="sxs-lookup"><span data-stu-id="00107-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="00107-111">Jednak może się okazać, że ma specjalizowany potrzeb routingu.</span><span class="sxs-lookup"><span data-stu-id="00107-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="00107-112">W takim przypadku można utworzyć tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="00107-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="00107-113">Załóżmy, na przykład tworzysz aplikację blogu.</span><span class="sxs-lookup"><span data-stu-id="00107-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="00107-114">Można obsłużyć żądań przychodzących, które wyglądają następująco:</span><span class="sxs-lookup"><span data-stu-id="00107-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="00107-115">/ Archiwum 12-25-2009</span><span class="sxs-lookup"><span data-stu-id="00107-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="00107-116">Gdy użytkownik wprowadza to żądanie, chcesz przywrócić wpis w blogu, która odpowiada Data 2009-12/25.</span><span class="sxs-lookup"><span data-stu-id="00107-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="00107-117">Aby obsługiwać ten typ żądania, należy utworzyć niestandardowe trasy.</span><span class="sxs-lookup"><span data-stu-id="00107-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="00107-118">Plik Global.asax 1 Lista zawiera nowe niestandardowe trasy, o nazwie blogu w żądaniach dojść, które mają postać /Archive/*Data wprowadzenia*.</span><span class="sxs-lookup"><span data-stu-id="00107-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="00107-119">**Wyświetlanie listy 1 - Global.asax (z tras niestandardowych)**</span><span class="sxs-lookup"><span data-stu-id="00107-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="00107-120">Kolejność trasy, które można dodać do tabeli tras jest ważna.</span><span class="sxs-lookup"><span data-stu-id="00107-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="00107-121">Nasze nowe niestandardowe trasy blogu zostanie dodany przed istniejącą trasę domyślną.</span><span class="sxs-lookup"><span data-stu-id="00107-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="00107-122">Jeśli odwrócić kolejność, następnie trasy domyślnej zawsze będzie uzyskać nazwę zamiast tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="00107-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="00107-123">Niestandardowe trasy blogu odpowiada każde żądanie, która rozpoczyna się od/archiwum /.</span><span class="sxs-lookup"><span data-stu-id="00107-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="00107-124">Tak jest on zgodny wszystkich następujących adresów URL:</span><span class="sxs-lookup"><span data-stu-id="00107-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="00107-125">/ Archiwum 12-25-2009</span><span class="sxs-lookup"><span data-stu-id="00107-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="00107-126">-Archiwum 10-6-2004</span><span class="sxs-lookup"><span data-stu-id="00107-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="00107-127">/ Archiwum/firmy apple</span><span class="sxs-lookup"><span data-stu-id="00107-127">/Archive/apple</span></span>

<span data-ttu-id="00107-128">Tras niestandardowych mapuje przychodzące żądanie do kontrolera o nazwie archiwum i wywołuje akcję Entry().</span><span class="sxs-lookup"><span data-stu-id="00107-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="00107-129">Gdy wywoływana jest metoda Entry(), daty rozpoczęcia jest przekazywana jako parametr o nazwie entryDate.</span><span class="sxs-lookup"><span data-stu-id="00107-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="00107-130">Można użyć tras niestandardowych blogu kontroler w wyświetlania 2.</span><span class="sxs-lookup"><span data-stu-id="00107-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="00107-131">**Wyświetlanie listy 2 - ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="00107-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="00107-132">Zwróć uwagę, że metoda Entry() wyświetlania 2 przyjmuje parametr typu DateTime.</span><span class="sxs-lookup"><span data-stu-id="00107-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="00107-133">Struktura MVC jest inteligentne przekonwertować datę wejścia z adresu URL na wartość daty i godziny automatycznie.</span><span class="sxs-lookup"><span data-stu-id="00107-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="00107-134">Jeśli parametr Data wpisu z adresu URL nie można przekonwertować na wartość typu DateTime, występuje błąd (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="00107-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="00107-135">**Rysunek 1 — błąd podczas konwersji parametru**</span><span class="sxs-lookup"><span data-stu-id="00107-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="00107-136">[![Okno dialogowe nowego projektu](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="00107-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="00107-137">**Rysunek 01**: błąd podczas konwersji parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="00107-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="00107-138">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="00107-138">Summary</span></span>

<span data-ttu-id="00107-139">Celem tego samouczka było wykazać, sposób tworzenia tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="00107-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="00107-140">Przedstawiono sposób dodawania tras niestandardowych do tabeli tras w pliku Global.asax, który reprezentuje wpisy.</span><span class="sxs-lookup"><span data-stu-id="00107-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="00107-141">Omówiono sposób mapowania żądania do wpisów w kontrolerze o nazwie ArchiveController i o nazwie Entry() akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="00107-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="00107-142">[Poprzednie](aspnet-mvc-controllers-overview-cs.md)
> [dalej](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="00107-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
