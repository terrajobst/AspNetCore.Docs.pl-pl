---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Tworzenie ograniczenia trasy (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: W tym samouczku Stephen Walther pokazano, jak można kontrolować, jak przeglądarka żąda trasy do dopasowania, tworząc ograniczenia trasy za pomocą wyrażeń regularnych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f50b371ac679218b06c4848e6d33516d29d3a82
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-route-constraint-vb"></a><span data-ttu-id="4f993-103">Tworzenie ograniczenia trasy (VB)</span><span class="sxs-lookup"><span data-stu-id="4f993-103">Creating a Route Constraint (VB)</span></span>
====================
<span data-ttu-id="4f993-104">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="4f993-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="4f993-105">W tym samouczku Stephen Walther pokazano, jak można kontrolować, jak przeglądarka żąda trasy do dopasowania, tworząc ograniczenia trasy za pomocą wyrażeń regularnych.</span><span class="sxs-lookup"><span data-stu-id="4f993-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="4f993-106">Ograniczenia trasy umożliwia ograniczenie żądań przeglądarki, które pasuje do określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="4f993-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="4f993-107">Wyrażenie regularne służy do określania ograniczenia trasy.</span><span class="sxs-lookup"><span data-stu-id="4f993-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="4f993-108">Na przykład załóżmy, że lista 1 zdefiniowano trasy w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="4f993-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="4f993-109">**Wyświetlanie listy 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="4f993-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="4f993-110">Wyświetlanie listy 1 zawiera trasy o nazwie produktu.</span><span class="sxs-lookup"><span data-stu-id="4f993-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="4f993-111">Trasy produktu służy do mapowania żądania przeglądarki ProductController zawarte w wyświetlania 2.</span><span class="sxs-lookup"><span data-stu-id="4f993-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="4f993-112">**Wyświetlanie listy 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="4f993-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="4f993-113">Zwróć uwagę, że akcja Details() udostępnianych przez kontroler produktu przyjmuje jeden parametr o nazwie productId.</span><span class="sxs-lookup"><span data-stu-id="4f993-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="4f993-114">Ten parametr jest parametru liczby całkowitej.</span><span class="sxs-lookup"><span data-stu-id="4f993-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="4f993-115">Trasy zdefiniowane w 1 Lista będzie pasuje do żadnego z następujących adresów URL:</span><span class="sxs-lookup"><span data-stu-id="4f993-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="4f993-116">/ Produktu/23</span><span class="sxs-lookup"><span data-stu-id="4f993-116">/Product/23</span></span>
- <span data-ttu-id="4f993-117">/ Produktu/7</span><span class="sxs-lookup"><span data-stu-id="4f993-117">/Product/7</span></span>

<span data-ttu-id="4f993-118">Niestety trasy również zgodna następujące adresy URL:</span><span class="sxs-lookup"><span data-stu-id="4f993-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="4f993-119">/ Produktu/blah</span><span class="sxs-lookup"><span data-stu-id="4f993-119">/Product/blah</span></span>
- <span data-ttu-id="4f993-120">/ Produktu/firmy apple</span><span class="sxs-lookup"><span data-stu-id="4f993-120">/Product/apple</span></span>

<span data-ttu-id="4f993-121">Ponieważ akcji Details() oczekuje parametru liczby całkowitej, wnioskiem zawiera inną niż wartość całkowitą spowoduje błąd.</span><span class="sxs-lookup"><span data-stu-id="4f993-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="4f993-122">Na przykład jeśli wpiszesz /Product/apple adres URL w przeglądarce następnie otrzymasz strony błędu na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="4f993-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="4f993-123">[![Okno dialogowe nowego projektu](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4f993-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="4f993-124">**Rysunek 01**: strona, rozwiń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4f993-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>


<span data-ttu-id="4f993-125">Co naprawdę chcesz zrobić to dopasowanie tylko adresów URL zawierających productId odpowiednie liczby całkowitej.</span><span class="sxs-lookup"><span data-stu-id="4f993-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="4f993-126">Umożliwia ograniczenie podczas definiowania trasy ograniczyć adresy URL zgodne trasy.</span><span class="sxs-lookup"><span data-stu-id="4f993-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="4f993-127">Zmodyfikowane trasy produktu w wersji 3 lista zawiera ograniczenie wyrażenie regularne, które jest zgodny tylko liczby całkowite.</span><span class="sxs-lookup"><span data-stu-id="4f993-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="4f993-128">**Wyświetlanie listy 3 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="4f993-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="4f993-129">\D+ wyrażenia regularnego odpowiada jednej lub więcej liczb całkowitych.</span><span class="sxs-lookup"><span data-stu-id="4f993-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="4f993-130">To ograniczenie powoduje, że produkt trasy do dopasowania następujące adresy URL:</span><span class="sxs-lookup"><span data-stu-id="4f993-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="4f993-131">/ Produkt/3</span><span class="sxs-lookup"><span data-stu-id="4f993-131">/Product/3</span></span>
- <span data-ttu-id="4f993-132">/ Produktu/8999</span><span class="sxs-lookup"><span data-stu-id="4f993-132">/Product/8999</span></span>

<span data-ttu-id="4f993-133">Ale nie następujących adresów URL:</span><span class="sxs-lookup"><span data-stu-id="4f993-133">But not the following URLs:</span></span>

- <span data-ttu-id="4f993-134">/ Produktu/firmy apple</span><span class="sxs-lookup"><span data-stu-id="4f993-134">/Product/apple</span></span>
- <span data-ttu-id="4f993-135">/ Produktu</span><span class="sxs-lookup"><span data-stu-id="4f993-135">/Product</span></span>

<span data-ttu-id="4f993-136">Te żądania przeglądarki będzie obsługiwany przez inny trasy lub, nie ma zgodnych tras *nie można odnaleźć zasobu* zostanie zwrócony błąd.</span><span class="sxs-lookup"><span data-stu-id="4f993-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4f993-137">[Poprzednie](creating-custom-routes-vb.md)
> [dalej](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4f993-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>
