---
title: Aktualizowanie wygenerowanego stron w aplikacji platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zaktualizować strony wygenerowane w aplikacji platformy ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 5/30/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: e36982f2b14ec65feb6be0c7f9e942c7a3c9e9ca
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34688435"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="0e814-103">Aktualizowanie wygenerowanego stron w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e814-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="0e814-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0e814-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0e814-105">Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="0e814-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="0e814-106">Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa wyrazy).</span><span class="sxs-lookup"><span data-stu-id="0e814-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="0e814-108">Aktualizacja wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="0e814-108">Update the generated code</span></span>

<span data-ttu-id="0e814-109">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="0e814-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="0e814-110">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="0e814-110">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="0e814-111">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]</span><span class="sxs-lookup"><span data-stu-id="0e814-111">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]</span></span>
::: moniker-end

<span data-ttu-id="0e814-112">Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red > **szybkie akcje i Refaktoryzacje**.</span><span class="sxs-lookup"><span data-stu-id="0e814-112">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](da1/qa.png)

<span data-ttu-id="0e814-114">Wybierz `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="0e814-114">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](da1/da.png)

  <span data-ttu-id="0e814-116">Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="0e814-116">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="0e814-117">[Poprzedni: Praca z bazy danych LocalDB programu SQL Server](xref:tutorials/razor-pages/sql)
> [Dodaj wyszukiwania](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="0e814-117">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
