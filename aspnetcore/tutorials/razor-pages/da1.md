---
title: Aktualizowanie stron wygenerowane w aplikacji ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zaktualizować wygenerowanych stron w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 78490be1cfa3018c465cb1e8125918404a7e4525
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011615"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="fe656-103">Aktualizowanie stron wygenerowane w aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe656-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="fe656-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fe656-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fe656-105">Mamy dobry początek aplikacji filmu, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="fe656-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="fe656-106">Nie chcemy wyświetlić czas (12:00:00 AM na ilustracji poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa słowa).</span><span class="sxs-lookup"><span data-stu-id="fe656-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji filmów](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="fe656-108">Aktualizacja wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="fe656-108">Update the generated code</span></span>

<span data-ttu-id="fe656-109">Otwórz *Models/Movie.cs* pliku i Dodaj wyróżnione wiersze pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="fe656-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="fe656-110">Kliknij prawym przyciskiem myszy czerwona linia falista > **szybkie akcje i Refaktoryzacje**.</span><span class="sxs-lookup"><span data-stu-id="fe656-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Pokazuje menu kontekstowe \*\* > Szybkie akcje i Refaktoryzacje \*\*.](da1/qa.png)

<span data-ttu-id="fe656-112">Wybierz pozycję `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="fe656-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![za pomocą System.ComponentModel.DataAnnotations u góry listy](da1/da.png)

  <span data-ttu-id="fe656-114">Program Visual studio dodaje `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="fe656-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="fe656-115">[Poprzedni: Praca z SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="fe656-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
