---
title: Metod kontrolera oraz widoki dla platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak pracować z metod kontrolera, widoków i DataAnnotations w ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 6fe0a0e71079bebcbd3a76abee0f2917f562e766
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="eda98-103">Metod kontrolera oraz widoki dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eda98-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="eda98-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eda98-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eda98-105">Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="eda98-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="eda98-106">Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinna być dwa wyrazy.</span><span class="sxs-lookup"><span data-stu-id="eda98-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](working-with-sql/_static/m55.png)

<span data-ttu-id="eda98-108">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="eda98-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="eda98-109">Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red **> Szybkie akcje i Refaktoryzacje**.</span><span class="sxs-lookup"><span data-stu-id="eda98-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](controller-methods-views/_static/qa.png)


<span data-ttu-id="eda98-111">Wybierz opcję `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="eda98-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](controller-methods-views/_static/da.png)

  <span data-ttu-id="eda98-113">Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="eda98-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="eda98-114">Umożliwia usunięcie `using` instrukcji, które nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="eda98-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="eda98-115">Są wyświetlane domyślnie światła szare czcionki.</span><span class="sxs-lookup"><span data-stu-id="eda98-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="eda98-116">Kliknij prawym przyciskiem myszy kliknij w dowolnym miejscu *Movie.cs* pliku **> Usuń i Sortuj deklaracje Using**.</span><span class="sxs-lookup"><span data-stu-id="eda98-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Usuń i Sortuj deklaracje Using](controller-methods-views/_static/rm.png)

<span data-ttu-id="eda98-118">Zaktualizowany kod:</span><span class="sxs-lookup"><span data-stu-id="eda98-118">The updated code:</span></span>

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="eda98-119">[Poprzednie](working-with-sql.md)
> [dalej](search.md)</span><span class="sxs-lookup"><span data-stu-id="eda98-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
