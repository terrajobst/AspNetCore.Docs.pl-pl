---
title: "Metody kontrolera i widoków"
author: rick-anderson
description: "Praca z metod kontrolera, widoków i DataAnnotations"
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-bb71-4adf-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: b72fb5077d3bd577cd6282e7a6231a0806ee1fd2
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2017
---
# <a name="controller-methods-and-views"></a>Metody kontrolera i widoków

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji film, ale nie jest idealnym rozwiązaniem prezentacji. Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinna być dwa wyrazy.

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

Skompiluj i uruchom aplikację.

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Poprzedni — Praca z SQLite](working-with-sql.md)
[następne — Dodaj wyszukiwania](search.md)  
