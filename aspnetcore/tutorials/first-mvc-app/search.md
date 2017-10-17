---
title: Dodawanie wyszukiwania
author: rick-anderson
description: "Przedstawiono sposób dodawania wyszukiwania do prostej aplikacji platformy ASP.NET Core MVC"
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-8ef6-4628-855d-200206d962b9
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: f811cd0063404872b0e993a99e8a92cc354064ce
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/11/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="9afd5-104">Można szybko zmienić `searchString` parametr `id` z **zmienić** polecenia.</span><span class="sxs-lookup"><span data-stu-id="9afd5-104">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="9afd5-105">Kliknij prawym przyciskiem myszy `searchString` **> Zmień nazwę**.</span><span class="sxs-lookup"><span data-stu-id="9afd5-105">Right click on `searchString` **> Rename**.</span></span>

![Menu kontekstowe](search/_static/rename.png)

<span data-ttu-id="9afd5-107">Elementy docelowe rename są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="9afd5-107">The rename targets are highlighted.</span></span>

![Edytor kodu przedstawiający zmiennej wyróżniane w metodzie ActionResult indeksu](search/_static/rename2.png)

<span data-ttu-id="9afd5-109">Zmień parametr `id` i wszystkie wystąpienia `searchString` Zmień `id`.</span><span class="sxs-lookup"><span data-stu-id="9afd5-109">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Edytor kodu przedstawiający zmiennej został zmieniony na identyfikator](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="9afd5-111">Zwróć uwagę, jak intelliSense pomaga nam zaktualizuj kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="9afd5-111">Notice how intelliSense helps us update the markup.</span></span>

![Menu kontekstowe IntelliSense z zaznaczony na liście atrybutów dla elementu form — metoda](search/_static/int_m.png)

![Menu kontekstowe IntelliSense z pobrać zaznaczony na liście wartości atrybutów — metoda](search/_static/int_get.png)

<span data-ttu-id="9afd5-114">Zwróć uwagę, charakterystyczne czcionkę w `<form>` tagu.</span><span class="sxs-lookup"><span data-stu-id="9afd5-114">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="9afd5-115">Charakterystyczne czcionki wskazuje tagu jest obsługiwana przez [pomocników tagów](../../mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="9afd5-115">That distinctive font indicates the tag is supported by [Tag Helpers](../../mvc/views/tag-helpers/intro.md).</span></span>

![tag formularza o purpurowa tekstu](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="9afd5-117">[Poprzednie](controller-methods-views.md)
[dalej](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="9afd5-117">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
