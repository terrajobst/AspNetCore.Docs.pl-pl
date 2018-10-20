---
title: Aktualizowanie stron wygenerowane w aplikacji ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zaktualizować wygenerowanych stron w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: f47a68840a6307b69bc92a7b157037d91dce5422
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477218"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Aktualizowanie stron wygenerowane w aplikacji ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji filmu, ale prezentacji nie jest najlepszym rozwiązaniem. Nie chcemy wyświetlić czas (12:00:00 AM na ilustracji poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa słowa).

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji filmów](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizacja wygenerowanego kodu

Otwórz *Models/Movie.cs* pliku i Dodaj wyróżnione wiersze pokazano w poniższym kodzie:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

Kliknij prawym przyciskiem myszy czerwona linia falista > **szybkie akcje i Refaktoryzacje**.

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](da1/qa.png)

Wybierz pozycję `using System.ComponentModel.DataAnnotations;`

  ![za pomocą System.ComponentModel.DataAnnotations u góry listy](da1/da.png)

  Program Visual Studio dodaje `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Poprzedni: Praca z SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [dalej: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
