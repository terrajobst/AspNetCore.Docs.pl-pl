---
title: Aktualizowanie wygenerowanego stron w aplikacji platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zaktualizować strony wygenerowane w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 55ff98712da314e28e50a1b1b1e04530d5b3fedd
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278073"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Aktualizowanie wygenerowanego stron w aplikacji platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem. Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa wyrazy).

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizacja wygenerowanego kodu

Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze poniższym kodem:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]
::: moniker-end

Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red > **szybkie akcje i Refaktoryzacje**.

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](da1/qa.png)

Wybierz `using System.ComponentModel.DataAnnotations;`

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](da1/da.png)

  Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Poprzedni: Praca z bazy danych LocalDB programu SQL Server](xref:tutorials/razor-pages/sql)
> [Dodaj wyszukiwania](xref:tutorials/razor-pages/search)
