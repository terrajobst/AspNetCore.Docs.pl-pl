---
title: Metody kontrolera i widoki w aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: "Praca z metod kontrolera, widoków i DataAnnotations"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 71cdf9f0a4a72f375af094c7c0a446278f8aeeb5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="84303-103">Metody kontrolera i widoki w aplikacji platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="84303-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="84303-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84303-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="84303-105">Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="84303-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="84303-106">Nie chcemy zobaczyć czas (00:00:00: 00 na poniższej ilustracji) i **ReleaseDate** powinna być dwa wyrazy.</span><span class="sxs-lookup"><span data-stu-id="84303-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="84303-108">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="84303-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="84303-109">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="84303-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="84303-110">[Poprzedni — Praca z SQLite](working-with-sql.md)
[następne — Dodaj wyszukiwania](search.md)</span><span class="sxs-lookup"><span data-stu-id="84303-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
