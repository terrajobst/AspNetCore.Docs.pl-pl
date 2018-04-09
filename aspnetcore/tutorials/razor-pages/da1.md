---
title: Aktualizowanie wygenerowanego stron w aplikacji platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zaktualizować strony wygenerowane w aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 0207696af14305a1b549cf55da1b42fa99d01776
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="4f3ea-103">Aktualizowanie wygenerowanego stron w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f3ea-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="4f3ea-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4f3ea-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4f3ea-105">Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="4f3ea-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="4f3ea-106">Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa wyrazy).</span><span class="sxs-lookup"><span data-stu-id="4f3ea-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="4f3ea-108">Aktualizacja wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="4f3ea-108">Update the generated code</span></span>

<span data-ttu-id="4f3ea-109">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="4f3ea-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="4f3ea-110">Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red > ** Szybkie akcje i Refaktoryzacje **.</span><span class="sxs-lookup"><span data-stu-id="4f3ea-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](da1/qa.png)

<span data-ttu-id="4f3ea-112">Wybierz `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="4f3ea-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](da1/da.png)

  <span data-ttu-id="4f3ea-114">Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="4f3ea-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="4f3ea-115">[Poprzedni: Praca z bazy danych LocalDB programu SQL Server](xref:tutorials/razor-pages/sql)
> [Dodaj wyszukiwania](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="4f3ea-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
