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
ms.openlocfilehash: 3f84d242a41bc482110d87ff342fa5b5d8c870ff
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729856"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="5f6f8-103">Metod kontrolera oraz widoki dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f6f8-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="5f6f8-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5f6f8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5f6f8-105">Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="5f6f8-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="5f6f8-106">Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinna być dwa wyrazy.</span><span class="sxs-lookup"><span data-stu-id="5f6f8-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](working-with-sql/_static/m55.png)

<span data-ttu-id="5f6f8-108">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="5f6f8-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="5f6f8-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span><span class="sxs-lookup"><span data-stu-id="5f6f8-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="5f6f8-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="5f6f8-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="5f6f8-111">[Poprzednie](working-with-sql.md)
> [dalej](search.md)</span><span class="sxs-lookup"><span data-stu-id="5f6f8-111">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
