---
title: Aktualizowanie wygenerowanego stron
author: rick-anderson
description: "Zaktualizuj stron wygenerowanych przy lepsze wyświetlanie."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a>Aktualizowanie wygenerowanego stron

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem. Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa wyrazy).

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizacja wygenerowanego kodu

Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze poniższym kodem:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red > ** Szybkie akcje i Refaktoryzacje **.

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](da1/qa.png)

Wybierz`using System.ComponentModel.DataAnnotations;`

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](da1/da.png)

  Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
[Poprzedni: Praca z bazy danych LocalDB programu SQL Server](xref:tutorials/razor-pages/sql)
[Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
