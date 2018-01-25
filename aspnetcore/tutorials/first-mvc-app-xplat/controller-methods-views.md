---
title: "Metody kontrolera i widoków"
author: rick-anderson
description: "Praca z metod kontrolera, widoków i DataAnnotations"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 38a91fc6cac4a07f0d30fd9aaa45e473f1283e7d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="5a72b-103">Metody kontrolera i widoków</span><span class="sxs-lookup"><span data-stu-id="5a72b-103">Controller methods and views</span></span>

<span data-ttu-id="5a72b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5a72b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5a72b-105">Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="5a72b-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="5a72b-106">Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinna być dwa wyrazy.</span><span class="sxs-lookup"><span data-stu-id="5a72b-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="5a72b-108">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="5a72b-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="5a72b-109">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="5a72b-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="5a72b-110">[Poprzedni — Praca z SQLite](working-with-sql.md)
[następne — Dodaj wyszukiwania](search.md)</span><span class="sxs-lookup"><span data-stu-id="5a72b-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
