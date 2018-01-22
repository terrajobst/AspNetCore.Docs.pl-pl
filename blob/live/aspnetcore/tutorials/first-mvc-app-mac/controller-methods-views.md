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
ms.openlocfilehash: 1c58a3bbac227e06a36df0b0d3ed6c2455eb46c4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="e3b77-103">Metody kontrolera i widoki w aplikacji platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e3b77-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="e3b77-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e3b77-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e3b77-105">Mamy dobry początek aplikacji film, ale nie jest idealnym rozwiązaniem prezentacji.</span><span class="sxs-lookup"><span data-stu-id="e3b77-105">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="e3b77-106">Nie chcemy zobaczyć czas (00:00:00: 00 na poniższej ilustracji) i **ReleaseDate** powinna być dwa wyrazy.</span><span class="sxs-lookup"><span data-stu-id="e3b77-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="e3b77-108">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="e3b77-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="e3b77-109">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="e3b77-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="e3b77-110">[Poprzedni — Praca z SQLite](working-with-sql.md)
[następne — Dodaj wyszukiwania](search.md)</span><span class="sxs-lookup"><span data-stu-id="e3b77-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
