---
title: "Metody kontrolera i widoków"
author: rick-anderson
description: "Praca z metod kontrolera, widoków i DataAnnotations"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 69f7bfa0f1e203c860abce06f6f2daffcaf47a59
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="controller-methods-and-views"></a>Metody kontrolera i widoków

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem. Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinna być dwa wyrazy.

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](working-with-sql/_static/m55.png)

Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red **> Szybkie akcje i Refaktoryzacje**.

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](controller-methods-views/_static/qa.png)


Wybierz opcję `using System.ComponentModel.DataAnnotations;`

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](controller-methods-views/_static/da.png)

  Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.

Umożliwia usunięcie `using` instrukcji, które nie są wymagane. Są wyświetlane domyślnie światła szare czcionki. Kliknij prawym przyciskiem myszy kliknij w dowolnym miejscu *Movie.cs* pliku **> Usuń i Sortuj deklaracje Using**.

![Usuń i Sortuj deklaracje Using](controller-methods-views/_static/rm.png)

Zaktualizowany kod:

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Poprzednie](working-with-sql.md)
[dalej](search.md)  
