---
title: Sprawdź szczegóły i metody zostaną usunięte z aplikacji platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat metody kontrolera szczegóły i wyświetlanie Podstawowa aplikacja platformy ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: b392f956888a740a4a8c7c553996fc85ce63bd4b
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729639"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a><span data-ttu-id="8c128-103">Sprawdź szczegóły i metody zostaną usunięte z aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c128-103">Examine the Details and Delete methods of an ASP.NET Core app</span></span>

<span data-ttu-id="8c128-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8c128-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8c128-105">Otwórz kontrolera filmu i sprawdź, czy `Details` metody:</span><span class="sxs-lookup"><span data-stu-id="8c128-105">Open the Movie controller and examine the `Details` method:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="8c128-106">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="8c128-106">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="8c128-107">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="8c128-107">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

::: moniker-end

<span data-ttu-id="8c128-108">Aparat szkieletów MVC utworzony tą metodą akcji dodaje komentarz przedstawiający żądanie HTTP, która wywołuje metodę.</span><span class="sxs-lookup"><span data-stu-id="8c128-108">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="8c128-109">W tym przypadku jest to żądanie GET z trzech segmenty adresu URL, `Movies` kontrolera, `Details` — metoda i `id` wartość.</span><span class="sxs-lookup"><span data-stu-id="8c128-109">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="8c128-110">Te segmenty są definiowane w odwołaniu *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8c128-110">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="8c128-111">EF ułatwia wyszukiwanie danych przy użyciu `SingleOrDefaultAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="8c128-111">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="8c128-112">Ważna funkcja zabezpieczeń wbudowanych w metodzie jest, czy kod sprawdza metody search znalazł filmu przed ponowną próbą podejmować żadnych działań z nim.</span><span class="sxs-lookup"><span data-stu-id="8c128-112">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="8c128-113">Na przykład haker może wprowadzić błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` podobną `http://localhost:xxxx/Movies/Details/12345` (lub inne wartości nie reprezentuje rzeczywisty film).</span><span class="sxs-lookup"><span data-stu-id="8c128-113">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="8c128-114">Jeśli nie wybierzesz null filmu aplikacji spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="8c128-114">If you didn't check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="8c128-115">Sprawdź `Delete` i `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="8c128-115">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="8c128-116">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="8c128-116">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="8c128-117">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="8c128-117">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

::: moniker-end

<span data-ttu-id="8c128-118">Należy pamiętać, że `HTTP GET Delete` — metoda nie powoduje usunięcia określonego filmu, zwraca widok filmu którego (HttpPost) można przesłać usunięcia.</span><span class="sxs-lookup"><span data-stu-id="8c128-118">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="8c128-119">Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub dla tej sprawy wykonywania operacji edycji, Utwórz operację lub innej operacji, które zmienia dane) otwiera luka w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="8c128-119">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="8c128-120">`[HttpPost]` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` umożliwiają metodą HTTP POST unikatowego podpisu lub nazwę.</span><span class="sxs-lookup"><span data-stu-id="8c128-120">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="8c128-121">Poniżej przedstawiono podpisy dwóch metod:</span><span class="sxs-lookup"><span data-stu-id="8c128-121">The two method signatures are shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="8c128-122">Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążonej metody ma unikatowy parametr podpisu (tej samej nazwy metody, ale lista różnych parametrów).</span><span class="sxs-lookup"><span data-stu-id="8c128-122">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="8c128-123">Jednak w tym miejscu należy dwa `Delete` metody — jeden dla GET - i jeden dla żądania POST, że mają taką samą sygnaturę parametru.</span><span class="sxs-lookup"><span data-stu-id="8c128-123">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="8c128-124">(Oba muszą zaakceptować pojedynczego całkowitą jako parametr.)</span><span class="sxs-lookup"><span data-stu-id="8c128-124">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="8c128-125">Istnieją dwa podejścia do tego problemu, co jest zapewniają różne nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="8c128-125">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="8c128-126">To mechanizm szkieletów został w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="8c128-126">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="8c128-127">Jednak powstaje mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy i zmiana metody routingu zwykle nie będą mogli odnaleźć tej metody.</span><span class="sxs-lookup"><span data-stu-id="8c128-127">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="8c128-128">Rozwiązanie, to zostanie wyświetlony w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="8c128-128">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="8c128-129">Ten atrybut wykonuje mapowanie systemu routingu, aby znaleźć adres URL, który zawiera /Delete/ dla żądania POST `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="8c128-129">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="8c128-130">Inny wspólnej obejść dla metod, które mają identyczne nazwy i podpisy jest sztucznie Zmiana podpisu metody POST, aby uwzględnić dodatkowy parametr (i nieużywanych).</span><span class="sxs-lookup"><span data-stu-id="8c128-130">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="8c128-131">To, co opisano w poprzedniej operacji publikowania podczas dodaliśmy `notUsed` parametru.</span><span class="sxs-lookup"><span data-stu-id="8c128-131">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="8c128-132">Można tak samo postąpić tutaj dla `[HttpPost] Delete` metody:</span><span class="sxs-lookup"><span data-stu-id="8c128-132">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="8c128-133">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="8c128-133">Publish to Azure</span></span>

<span data-ttu-id="8c128-134">Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące sposobu publikowania tej aplikacji na platformie Azure przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8c128-134">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="8c128-135">Aplikacja może również być publikowane z [wiersza polecenia](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="8c128-135">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8c128-136">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="8c128-136">Previous</span></span>](validation.md)
