---
title: Metody kontrolera i widoki w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak pracować z metody kontrolera, widoków i DataAnnotations w programie ASP.NET Core.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 42a63044cd14873ff334a728c6c8304214ee8575
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011342"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>Metody kontrolera i widoki w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji filmu, ale prezentacji nie jest najlepszym rozwiązaniem. Nie chcemy wyświetlić czas (12:00:00 AM na ilustracji poniżej) i **ReleaseDate** powinna być dwa słowa.

![Indeks widoku: Data wydania jest jedno słowo (bez spacji) i każdy film Data wydania zawiera godzinę 12: 00](working-with-sql/_static/m55.png)

Otwórz *Models/Movie.cs* pliku i Dodaj wyróżnione wiersze, które przedstawiono poniżej:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Poprzednie](working-with-sql.md)
> [dalej](search.md)  
