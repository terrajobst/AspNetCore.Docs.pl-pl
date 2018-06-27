---
title: Dodawanie do aplikacji ASP.NET Core MVC wyszukiwania
author: rick-anderson
description: Przedstawiono sposób dodawania wyszukiwania do prostej aplikacji platformy ASP.NET Core MVC
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960843"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

<span data-ttu-id="eb35e-103">Można szybko zmienić `searchString` parametr `id` z **zmienić** polecenia.</span><span class="sxs-lookup"><span data-stu-id="eb35e-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="eb35e-104">Kliknij prawym przyciskiem myszy `searchString` **> Zmień nazwę**.</span><span class="sxs-lookup"><span data-stu-id="eb35e-104">Right click on `searchString` **> Rename**.</span></span>

![Menu kontekstowe](search/_static/rename.png)

<span data-ttu-id="eb35e-106">Elementy docelowe rename są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="eb35e-106">The rename targets are highlighted.</span></span>

![Edytor kodu przedstawiający zmiennej wyróżniane w metodzie ActionResult indeksu](search/_static/rename2.png)

<span data-ttu-id="eb35e-108">Zmień parametr `id` i wszystkie wystąpienia `searchString` Zmień `id`.</span><span class="sxs-lookup"><span data-stu-id="eb35e-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Edytor kodu przedstawiający zmiennej został zmieniony na identyfikator](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="eb35e-110">Zwróć uwagę, jak intelliSense pomaga nam zaktualizuj kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="eb35e-110">Notice how intelliSense helps us update the markup.</span></span>

![Menu kontekstowe IntelliSense z zaznaczony na liście atrybutów dla elementu form — metoda](search/_static/int_m.png)

![Menu kontekstowe IntelliSense z pobrać zaznaczony na liście wartości atrybutów — metoda](search/_static/int_get.png)

<span data-ttu-id="eb35e-113">Zwróć uwagę, charakterystyczne czcionkę w `<form>` tagu.</span><span class="sxs-lookup"><span data-stu-id="eb35e-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="eb35e-114">Charakterystyczne czcionki wskazuje tagu jest obsługiwana przez [pomocników tagów](~/mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="eb35e-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![tag formularza o purpurowa tekstu](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="eb35e-116">[Poprzednie](controller-methods-views.md)
> [dalej](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="eb35e-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
