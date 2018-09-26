---
title: Sprawdź szczegóły i usunięcie metod aplikacji ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o szczegóły metody kontrolera i wyświetlanie w podstawowej aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 9864abb54483c0ccf911aaf704a1beae007b32a4
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47211016"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a><span data-ttu-id="8834b-103">Sprawdź szczegóły i usunięcie metod aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8834b-103">Examine the Details and Delete methods of an ASP.NET Core app</span></span>

<span data-ttu-id="8834b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8834b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8834b-105">Otwórz kontrolera film i zbadaj `Details` metody:</span><span class="sxs-lookup"><span data-stu-id="8834b-105">Open the Movie controller and examine the `Details` method:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

::: moniker-end

<span data-ttu-id="8834b-106">Aparat tworzenia szkieletów MVC, utworzony z tą metodą akcji dodaje komentarz przedstawiający żądanie HTTP, który wywołuje tę metodę.</span><span class="sxs-lookup"><span data-stu-id="8834b-106">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="8834b-107">W tym przypadku jest żądanie GET z trzech segmenty adresu URL, `Movies` kontrolera, `Details` metody i `id` wartość.</span><span class="sxs-lookup"><span data-stu-id="8834b-107">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="8834b-108">Te segmenty są definiowane w odwołania *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8834b-108">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="8834b-109">EF ułatwia wyszukiwanie danych przy użyciu `SingleOrDefaultAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="8834b-109">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="8834b-110">Ważna funkcja zabezpieczeń wbudowanych w metodzie jest kod sprawdza, ta metoda wyszukiwania wykryła filmu przed ponowną próbą podejmować żadnych działań z nim.</span><span class="sxs-lookup"><span data-stu-id="8834b-110">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="8834b-111">Na przykład haker może spowodować błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` na wartość podobną `http://localhost:xxxx/Movies/Details/12345` (lub inną wartość, która nie zawiera rzeczywistych filmu).</span><span class="sxs-lookup"><span data-stu-id="8834b-111">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="8834b-112">Jeśli zaznaczono film o wartości null aplikacji będzie zgłaszają wyjątek.</span><span class="sxs-lookup"><span data-stu-id="8834b-112">If you didn't check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="8834b-113">Sprawdź `Delete` i `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="8834b-113">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

::: moniker-end

<span data-ttu-id="8834b-114">Należy pamiętać, że `HTTP GET Delete` metoda nie powoduje usunięcia określonego filmu, zwraca widok filmu dokąd wysyłać (HttpPost) usuwania.</span><span class="sxs-lookup"><span data-stu-id="8834b-114">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="8834b-115">Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub służącego wykonywania operacji Edytuj, Utwórz operacji lub innej operacji, które zmieniają dane) otwiera lukę w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="8834b-115">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="8834b-116">`[HttpPost]` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` zapewnienie metodą HTTP POST unikatowy podpis lub nazwy.</span><span class="sxs-lookup"><span data-stu-id="8834b-116">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="8834b-117">Poniżej przedstawiono podpisy dwóch metod:</span><span class="sxs-lookup"><span data-stu-id="8834b-117">The two method signatures are shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="8834b-118">Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążonej metody ma unikatowy parametr podpisu (tej samej nazwie metoda, ale inną listę parametrów).</span><span class="sxs-lookup"><span data-stu-id="8834b-118">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="8834b-119">Jednak w tym miejscu należy dwa `Delete` metody — jeden dla GET--i jeden dla wpisu, czy obie pozycje mają taki sam podpis parametru.</span><span class="sxs-lookup"><span data-stu-id="8834b-119">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="8834b-120">(Obaj użytkownicy muszą zaakceptować pojedyncze liczby całkowite jako parametr.)</span><span class="sxs-lookup"><span data-stu-id="8834b-120">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="8834b-121">Dostępne są dwie opcje tego problemu, jednym jest nadać różne nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="8834b-121">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="8834b-122">To mechanizm tworzenia szkieletów została w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="8834b-122">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="8834b-123">Jednak wprowadza mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy i metodę w przypadku zmiany nazwy, routing zwykle nie będzie mógł odnaleźć tej metody.</span><span class="sxs-lookup"><span data-stu-id="8834b-123">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="8834b-124">Rozwiązanie jest widoczny w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="8834b-124">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="8834b-125">Ten atrybut wykonuje mapowanie systemu routingu, aby znaleźć adres URL, który zawiera /Delete/ dla żądania POST `DeleteConfirmed` metody.</span><span class="sxs-lookup"><span data-stu-id="8834b-125">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="8834b-126">Inny wspólnej obejście dla metod, które mają identyczne nazwy i wzory podpisów jest sztucznie Zmień podpis metody POST w celu uwzględnienia dodatkowych parametrów (nieużywane).</span><span class="sxs-lookup"><span data-stu-id="8834b-126">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="8834b-127">To, co zrobiliśmy w poprzednim wpisie podczas dodaliśmy `notUsed` parametru.</span><span class="sxs-lookup"><span data-stu-id="8834b-127">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="8834b-128">Możesz to zrobić to samo w tym miejscu dla `[HttpPost] Delete` metody:</span><span class="sxs-lookup"><span data-stu-id="8834b-128">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="8834b-129">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="8834b-129">Publish to Azure</span></span>

<span data-ttu-id="8834b-130">Aby uzyskać informacje na temat wdrażania na platformie Azure, zobacz [samouczek: tworzenie aplikacji ASP.NET na platformie Azure przy użyciu bazy danych SQL](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span><span class="sxs-lookup"><span data-stu-id="8834b-130">For information on deploying to Azure, See [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="8834b-131">Instrukcje są przeznaczone dla aplikacji platformy ASP.NET, nie aplikacji ASP.NET Core, ale kroki są takie same.</span><span class="sxs-lookup"><span data-stu-id="8834b-131">The instruction are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8834b-132">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="8834b-132">Previous</span></span>](validation.md)
