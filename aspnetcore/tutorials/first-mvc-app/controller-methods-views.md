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
# <a name="controller-methods-and-views"></a><span data-ttu-id="6b16e-103">Metody kontrolera i widoków</span><span class="sxs-lookup"><span data-stu-id="6b16e-103">Controller methods and views</span></span>

<span data-ttu-id="6b16e-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6b16e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6b16e-105">Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="6b16e-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="6b16e-106">Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinna być dwa wyrazy.</span><span class="sxs-lookup"><span data-stu-id="6b16e-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](working-with-sql/_static/m55.png)

<span data-ttu-id="6b16e-108">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="6b16e-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="6b16e-109">Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red **> Szybkie akcje i Refaktoryzacje**.</span><span class="sxs-lookup"><span data-stu-id="6b16e-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](controller-methods-views/_static/qa.png)


<span data-ttu-id="6b16e-111">Wybierz opcję `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="6b16e-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](controller-methods-views/_static/da.png)

  <span data-ttu-id="6b16e-113">Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="6b16e-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="6b16e-114">Umożliwia usunięcie `using` instrukcji, które nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="6b16e-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="6b16e-115">Są wyświetlane domyślnie światła szare czcionki.</span><span class="sxs-lookup"><span data-stu-id="6b16e-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="6b16e-116">Kliknij prawym przyciskiem myszy kliknij w dowolnym miejscu *Movie.cs* pliku **> Usuń i Sortuj deklaracje Using**.</span><span class="sxs-lookup"><span data-stu-id="6b16e-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Usuń i Sortuj deklaracje Using](controller-methods-views/_static/rm.png)

<span data-ttu-id="6b16e-118">Zaktualizowany kod:</span><span class="sxs-lookup"><span data-stu-id="6b16e-118">The updated code:</span></span>

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="6b16e-119">[Poprzednie](working-with-sql.md)
[dalej](search.md)</span><span class="sxs-lookup"><span data-stu-id="6b16e-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
