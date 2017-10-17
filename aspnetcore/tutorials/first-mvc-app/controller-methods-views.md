---
title: "Metody kontrolera i widoków"
author: rick-anderson
description: "Praca z metod kontrolera, widoków i DataAnnotations"
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a>Metody kontrolera i widoków

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji film, ale nie jest idealnym rozwiązaniem prezentacji. Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinna być dwa wyrazy.

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](working-with-sql/_static/m55.png)

Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red **> Szybkie akcje i Refaktoryzacje**.

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](controller-methods-views/_static/qa.png)


Wybierz opcję`using System.ComponentModel.DataAnnotations;`

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](controller-methods-views/_static/da.png)

  Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.

Umożliwia usunięcie `using` instrukcji, które nie są wymagane. Są wyświetlane domyślnie światła szare czcionki. Kliknij prawym przyciskiem myszy kliknij w dowolnym miejscu *Movie.cs* pliku **> Usuń i Sortuj deklaracje Using**.

![Usuń i Sortuj deklaracje Using](controller-methods-views/_static/rm.png)

Zaktualizowany kod:

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Poprzednie](working-with-sql.md)
[dalej](search.md)  
