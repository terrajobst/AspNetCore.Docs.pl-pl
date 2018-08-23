---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Tworzenie tras niestandardowych (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać trasy niestandardowe do aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak zmodyfikować tabela routingu domyślnego w pliku Global.asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: cd7ad46161b3f44d915bc4b2fa201606b00d194a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754977"
---
<a name="creating-custom-routes-vb"></a><span data-ttu-id="71937-104">Tworzenie tras niestandardowych (VB)</span><span class="sxs-lookup"><span data-stu-id="71937-104">Creating Custom Routes (VB)</span></span>
====================
<span data-ttu-id="71937-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="71937-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="71937-106">Dowiedz się, jak dodać trasy niestandardowe do aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="71937-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="71937-107">W tym samouczku dowiesz się, jak zmodyfikować tabela routingu domyślnego w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="71937-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="71937-108">W tym samouczku dowiesz się, jak dodać trasy niestandardowej do aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="71937-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="71937-109">Dowiesz się, jak zmodyfikować tabela routingu domyślnego w pliku Global.asax z tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="71937-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="71937-110">W aplikacjach ASP.NET MVC tabela routingu domyślnego będzie działać prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="71937-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="71937-111">Jednak może się okazać, że ma wyspecjalizowany potrzeb routingu.</span><span class="sxs-lookup"><span data-stu-id="71937-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="71937-112">W takim przypadku można utworzyć trasy niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="71937-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="71937-113">Wyobraź sobie, na przykład tworzysz aplikację blogu.</span><span class="sxs-lookup"><span data-stu-id="71937-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="71937-114">Możesz chcieć obsłużyć żądań przychodzących, które wyglądają następująco:</span><span class="sxs-lookup"><span data-stu-id="71937-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="71937-115">/ Archiwum/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="71937-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="71937-116">Kiedy użytkownik wprowadzi tego żądania, mają być zwracane wpis w blogu, który odnosi się do wartości typu date 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="71937-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="71937-117">Aby umożliwić obsługę tego typu żądania, musisz utworzyć tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="71937-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="71937-118">Plik Global.asax w ofercie 1 zawiera nowe trasy niestandardowej o nazwie blogu, które obsługuje żądania, które mają postać /Archive/*Data wpisu*.</span><span class="sxs-lookup"><span data-stu-id="71937-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="71937-119">**Wyświetlanie listy 1 - Global.asax (przy użyciu tras niestandardowych)**</span><span class="sxs-lookup"><span data-stu-id="71937-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="71937-120">Kolejność trasy, które można dodać do tabeli tras jest ważna.</span><span class="sxs-lookup"><span data-stu-id="71937-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="71937-121">Nasz nowy trasy niestandardowej blogu zostanie dodany przed istniejącą trasę domyślną.</span><span class="sxs-lookup"><span data-stu-id="71937-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="71937-122">Czy to zamówienie, jest wycofywany, a następnie trasy domyślnej zawsze ma zostać wywołana zamiast tras niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="71937-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="71937-123">Każde żądanie, które zaczyna się od/archiwum/pasuje do trasy niestandardowej blogu.</span><span class="sxs-lookup"><span data-stu-id="71937-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="71937-124">Tak jest on zgodny wszystkie następujące adresy URL:</span><span class="sxs-lookup"><span data-stu-id="71937-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="71937-125">/ Archiwum/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="71937-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="71937-126">/ Archiwum/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="71937-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="71937-127">/ Archiwum/firmy apple</span><span class="sxs-lookup"><span data-stu-id="71937-127">/Archive/apple</span></span>

<span data-ttu-id="71937-128">Trasy niestandardowej mapy przychodzące żądanie do kontrolera, o nazwie archiwum i wywołuje akcję Entry().</span><span class="sxs-lookup"><span data-stu-id="71937-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="71937-129">Gdy wywoływana jest metoda Entry(), daty rozpoczęcia jest przekazywany jako parametr o nazwie entryDate.</span><span class="sxs-lookup"><span data-stu-id="71937-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="71937-130">Za pomocą tras niestandardowych blogu kontrolera w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="71937-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="71937-131">**Wyświetlanie listy 2 - ArchiveController.vb**</span><span class="sxs-lookup"><span data-stu-id="71937-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="71937-132">Należy zauważyć, że metoda Entry() w ofercie 2 akceptuje parametr typu Data/Godzina.</span><span class="sxs-lookup"><span data-stu-id="71937-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="71937-133">Struktura MVC jest inteligentnego przekonwertować datę wejścia z adresu URL na wartość typu DateTime automatycznie.</span><span class="sxs-lookup"><span data-stu-id="71937-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="71937-134">Jeśli parametr daty wpis z adresu URL nie można przekonwertować na wartość typu DateTime, zgłaszany jest błąd (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="71937-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="71937-135">**Rysunek 1. Błąd konwersji parametru**</span><span class="sxs-lookup"><span data-stu-id="71937-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="71937-136">[![Okno dialogowe Nowy projekt](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="71937-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="71937-137">**Rysunek 01**: błąd konwersji parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-routes-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="71937-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="71937-138">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="71937-138">Summary</span></span>

<span data-ttu-id="71937-139">Celem tego samouczka było pokazują, jak można utworzyć trasy niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="71937-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="71937-140">Pokazaliśmy ci, jak dodać niestandardowe trasy w tabeli tras w pliku Global.asax, który reprezentuje wpisy w blogu.</span><span class="sxs-lookup"><span data-stu-id="71937-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="71937-141">Omówiliśmy sposób mapowania żądania dotyczące wpisów w blogu o nazwie ArchiveController kontrolera i akcji kontrolera, o nazwie Entry().</span><span class="sxs-lookup"><span data-stu-id="71937-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="71937-142">[Poprzednie](asp-net-mvc-controller-overview-vb.md)
> [dalej](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="71937-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
