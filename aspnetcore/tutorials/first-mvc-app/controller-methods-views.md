---
title: Metod kontrolera oraz widoki dla platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak pracować z metod kontrolera, widoków i DataAnnotations w ASP.NET Core.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: e94cb877576a68540a565225b2b3d79f9be53327
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274817"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="eb983-103">Metod kontrolera oraz widoki dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eb983-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="eb983-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eb983-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eb983-105">Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="eb983-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="eb983-106">Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinna być dwa wyrazy.</span><span class="sxs-lookup"><span data-stu-id="eb983-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](working-with-sql/_static/m55.png)

<span data-ttu-id="eb983-108">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="eb983-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="eb983-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span><span class="sxs-lookup"><span data-stu-id="eb983-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="eb983-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="eb983-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="eb983-111">[Poprzednie](working-with-sql.md)
> [dalej](search.md)</span><span class="sxs-lookup"><span data-stu-id="eb983-111">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
