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
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="2eef0-104">Metody kontrolera i widoków</span><span class="sxs-lookup"><span data-stu-id="2eef0-104">Controller methods and views</span></span>

<span data-ttu-id="2eef0-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2eef0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2eef0-106">Mamy dobry początek aplikacji film, ale nie jest idealnym rozwiązaniem prezentacji.</span><span class="sxs-lookup"><span data-stu-id="2eef0-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="2eef0-107">Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinna być dwa wyrazy.</span><span class="sxs-lookup"><span data-stu-id="2eef0-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Indeksuj zawartości widoku: Data wydania jest jednego słowa (bez spacji) i co data wydania film zawiera godzinę 00: 00](working-with-sql/_static/m55.png)

<span data-ttu-id="2eef0-109">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze, pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="2eef0-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="2eef0-110">Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red **> Szybkie akcje i Refaktoryzacje**.</span><span class="sxs-lookup"><span data-stu-id="2eef0-110">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](controller-methods-views/_static/qa.png)


<span data-ttu-id="2eef0-112">Wybierz opcję`using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="2eef0-112">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](controller-methods-views/_static/da.png)

  <span data-ttu-id="2eef0-114">Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="2eef0-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="2eef0-115">Umożliwia usunięcie `using` instrukcji, które nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="2eef0-115">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="2eef0-116">Są wyświetlane domyślnie światła szare czcionki.</span><span class="sxs-lookup"><span data-stu-id="2eef0-116">They show up by default in a light grey font.</span></span> <span data-ttu-id="2eef0-117">Kliknij prawym przyciskiem myszy kliknij w dowolnym miejscu *Movie.cs* pliku **> Usuń i Sortuj deklaracje Using**.</span><span class="sxs-lookup"><span data-stu-id="2eef0-117">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Usuń i Sortuj deklaracje Using](controller-methods-views/_static/rm.png)

<span data-ttu-id="2eef0-119">Zaktualizowany kod:</span><span class="sxs-lookup"><span data-stu-id="2eef0-119">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="2eef0-120">[Poprzednie](working-with-sql.md)
[dalej](search.md)</span><span class="sxs-lookup"><span data-stu-id="2eef0-120">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
