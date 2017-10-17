---
title: "Badanie szczegóły i metody zostaną usunięte"
author: rick-anderson
description: "Szczegóły metody kontrolera i widoku w prostej aplikacji ASP.NET Core MVC."
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: 870192b4-8d4f-45c7-8c14-83d02bc0ad79
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 28ed7a7a56415d7eb675c06353fb9a8f65fb571f
ms.sourcegitcommit: c9658c0db446f7cb2e443f62b00cf773bed545fa
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/30/2017
---
# <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="7d5bf-104">Badanie szczegóły i metody zostaną usunięte</span><span class="sxs-lookup"><span data-stu-id="7d5bf-104">Examining the Details and Delete methods</span></span>

<span data-ttu-id="7d5bf-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d5bf-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d5bf-106">Otwórz kontrolera filmu i sprawdź, czy `Details` metody:</span><span class="sxs-lookup"><span data-stu-id="7d5bf-106">Open the Movie controller and examine the `Details` method:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="7d5bf-107">Aparat szkieletów MVC utworzony tą metodą akcji dodaje komentarz przedstawiający żądanie HTTP, która wywołuje metodę.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-107">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="7d5bf-108">W tym przypadku jest to żądanie GET z trzech segmenty adresu URL, `Movies` kontrolera, `Details` — metoda i `id` wartość.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-108">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="7d5bf-109">Te segmenty są definiowane w odwołaniu *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-109">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="7d5bf-110">EF ułatwia wyszukiwanie danych przy użyciu `SingleOrDefaultAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-110">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="7d5bf-111">Ważna funkcja zabezpieczeń wbudowanych w metodzie jest, czy kod sprawdza metody search znalazł filmu przed ponowną próbą podejmować żadnych działań z nim.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-111">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="7d5bf-112">Na przykład haker może wprowadzić błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` podobną `http://localhost:xxxx/Movies/Details/12345` (lub inne wartości nie reprezentuje rzeczywisty film).</span><span class="sxs-lookup"><span data-stu-id="7d5bf-112">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="7d5bf-113">Jeśli nie zaznaczono filmu wartości null, aplikacji spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-113">If you did not check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="7d5bf-114">Sprawdź `Delete` i `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-114">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

<span data-ttu-id="7d5bf-115">Należy pamiętać, że `HTTP GET Delete` — metoda nie powoduje usunięcia określonego filmu, zwraca widok filmu którego (HttpPost) można przesłać usunięcia.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-115">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="7d5bf-116">Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub dla tej sprawy wykonywania operacji edycji, Utwórz operację lub innej operacji, które zmienia dane) otwiera luka w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-116">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="7d5bf-117">`[HttpPost]` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` umożliwiają metodą HTTP POST unikatowego podpisu lub nazwę.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-117">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="7d5bf-118">Poniżej przedstawiono podpisy dwóch metod:</span><span class="sxs-lookup"><span data-stu-id="7d5bf-118">The two method signatures are shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="7d5bf-119">Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążonej metody ma unikatowy parametr podpisu (tej samej nazwy metody, ale lista różnych parametrów).</span><span class="sxs-lookup"><span data-stu-id="7d5bf-119">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="7d5bf-120">Jednak w tym miejscu należy dwa `Delete` metody — jeden dla GET - i jeden dla żądania POST, że mają taką samą sygnaturę parametru.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-120">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="7d5bf-121">(Oba muszą zaakceptować pojedynczego całkowitą jako parametr.)</span><span class="sxs-lookup"><span data-stu-id="7d5bf-121">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="7d5bf-122">Istnieją dwa podejścia do tego problemu, co jest zapewniają różne nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-122">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="7d5bf-123">To mechanizm szkieletów został w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-123">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="7d5bf-124">Jednak powstaje mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy i zmiana metody routingu zwykle nie będą mogli odnaleźć tej metody.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-124">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="7d5bf-125">Rozwiązanie, to zostanie wyświetlony w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-125">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="7d5bf-126">Ten atrybut wykonuje mapowanie systemu routingu, aby znaleźć adres URL, który zawiera /Delete/ dla żądania POST `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-126">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="7d5bf-127">Inny wspólnej obejść dla metod, które mają identyczne nazwy i podpisy jest sztucznie Zmiana podpisu metody POST, aby uwzględnić dodatkowy parametr (i nieużywanych).</span><span class="sxs-lookup"><span data-stu-id="7d5bf-127">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="7d5bf-128">To, co opisano w poprzedniej operacji publikowania podczas dodaliśmy `notUsed` parametru.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-128">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="7d5bf-129">Można tak samo postąpić tutaj dla `[HttpPost] Delete` metody:</span><span class="sxs-lookup"><span data-stu-id="7d5bf-129">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="7d5bf-130">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="7d5bf-130">Publish to Azure</span></span>

<span data-ttu-id="7d5bf-131">Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące sposobu publikowania tej aplikacji na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-131">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure.</span></span>

<span data-ttu-id="7d5bf-132">Dziękujemy za korzystanie to wprowadzenie do platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-132">Thanks for completing this introduction to ASP.NET Core MVC.</span></span> <span data-ttu-id="7d5bf-133">Dziękujemy za wszelkie komentarze, które pozostaną.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-133">We appreciate any comments you leave.</span></span> <span data-ttu-id="7d5bf-134">[Wprowadzenie do programu MVC i podstawowe EF](xref:data/ef-mvc/intro) jest doskonałym uzupełnianie w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="7d5bf-134">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7d5bf-135">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="7d5bf-135">Previous</span></span>](validation.md)
