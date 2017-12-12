---
title: Metody kontrolera i widoki w aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: "Praca z metod kontrolera, widoków i DataAnnotations"
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-babe-babe-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 846a78a09cdde0cfdbcf35bb899c1822ebe8fea2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="3e2ae-104">Metody kontrolera i widoki w aplikacji platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="3e2ae-104">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="3e2ae-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3e2ae-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3e2ae-106">Mamy dobry początek aplikacji film, ale nie jest idealnym rozwiązaniem prezentacji.</span><span class="sxs-lookup"><span data-stu-id="3e2ae-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="3e2ae-107">Nie chcemy zobaczyć czas (00:00:00: 00 na poniższej ilustracji) i **ReleaseDate** powinna być dwa wyrazy.</span><span class="sxs-lookup"><span data-stu-id="3e2ae-107">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="3e2ae-109">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="3e2ae-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="3e2ae-110">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="3e2ae-110">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="3e2ae-111">[Poprzedni — Praca z SQLite](working-with-sql.md)
[następne — Dodaj wyszukiwania](search.md)</span><span class="sxs-lookup"><span data-stu-id="3e2ae-111">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
