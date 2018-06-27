---
title: Dodawanie do aplikacji ASP.NET Core MVC wyszukiwania
author: rick-anderson
description: Przedstawiono sposób dodawania wyszukiwania do prostej aplikacji platformy ASP.NET Core MVC
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: cf4fe3806b45008f48bf5f0598057552bdcfae7c
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961441"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="43a78-103">Uwaga: SQLlite jest uwzględniana wielkość liter, więc należy wyszukać "Duplikatu", a nie "duplikatu".</span><span class="sxs-lookup"><span data-stu-id="43a78-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="43a78-104">Zmień `<form>` tagów w *Views\movie\Index.cshtml* widoku Razor, aby określić `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="43a78-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="43a78-105">[Poprzedni - metod kontrolera oraz widoki](controller-methods-views.md)
> [następne — Dodawanie pola](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="43a78-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>  
