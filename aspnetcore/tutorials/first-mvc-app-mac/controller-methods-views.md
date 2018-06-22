---
title: Metody kontrolera i widoki w aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: Praca z metod kontrolera, widoków i DataAnnotations
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 0bd57597c74918829de38764326e35df08ab1e05
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279080"
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a>Metody kontrolera i widoki w aplikacji platformy ASP.NET Core MVC

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem. Nie chcemy zobaczyć czas (00:00:00: 00 na poniższej ilustracji) i **ReleaseDate** powinna być dwa wyrazy.

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

Skompiluj i uruchom aplikację.

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Poprzedni — Praca z SQLite](working-with-sql.md)
> [następne — Dodaj wyszukiwania](search.md)
