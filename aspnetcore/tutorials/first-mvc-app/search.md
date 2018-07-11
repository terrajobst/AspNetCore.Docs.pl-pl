---
title: Dodawanie wyszukiwania do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Pokazuje, jak dodawanie wyszukiwania do prostą aplikację platformy ASP.NET Core MVC
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216199"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

<span data-ttu-id="f3e45-103">Możesz szybko zmienić nazwę `searchString` parametr `id` z **Zmień nazwę** polecenia.</span><span class="sxs-lookup"><span data-stu-id="f3e45-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="f3e45-104">Kliknij prawym przyciskiem myszy `searchString` **> Zmień nazwę**.</span><span class="sxs-lookup"><span data-stu-id="f3e45-104">Right click on `searchString` **> Rename**.</span></span>

![Menu kontekstowe](search/_static/rename.png)

<span data-ttu-id="f3e45-106">Obiekty docelowe zmiany nazwy są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="f3e45-106">The rename targets are highlighted.</span></span>

![Edytor kodu, przedstawiający zmiennej wyróżnione w całym metoda ActionResult indeksu](search/_static/rename2.png)

<span data-ttu-id="f3e45-108">Zmień parametr `id` i wszystkie wystąpienia `searchString` Zmień `id`.</span><span class="sxs-lookup"><span data-stu-id="f3e45-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Edytor kodu, przedstawiający zmiennej został zmieniony na identyfikator](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="f3e45-110">Zwróć uwagę, jak technologia intelliSense pomaga zaktualizować znaczników.</span><span class="sxs-lookup"><span data-stu-id="f3e45-110">Notice how intelliSense helps us update the markup.</span></span>

![Menu kontekstowe funkcji IntelliSense przy użyciu metody zaznaczony na liście atrybutów dla elementu form](search/_static/int_m.png)

![Menu kontekstowe IntelliSense z pobrać zaznaczony na liście wartości atrybutów — metoda](search/_static/int_get.png)

<span data-ttu-id="f3e45-113">Zwróć uwagę, szczególne czcionkę w `<form>` tagu.</span><span class="sxs-lookup"><span data-stu-id="f3e45-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="f3e45-114">Szczególne czcionki wskazuje tagu jest obsługiwana przez [pomocników tagów](~/mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="f3e45-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![tag formularza z tekstem purpurowy](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="f3e45-116">[Poprzednie](controller-methods-views.md)
> [dalej](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="f3e45-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
