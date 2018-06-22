---
title: Dodawanie wyszukiwania do platformy ASP.NET Core aplikacji MVC
author: rick-anderson
description: Przedstawiono sposób dodawania wyszukiwania do prostej aplikacji platformy ASP.NET Core MVC
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 4175d4dfd03d173f7025aff3b51d255bb1c213ee
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274485"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="6282c-103">Uwaga: SQLlite jest uwzględniana wielkość liter, więc należy wyszukać "Duplikatu", a nie "duplikatu".</span><span class="sxs-lookup"><span data-stu-id="6282c-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="6282c-104">Zmień `<form>` tagów w *Views\movie\Index.cshtml* widoku Razor, aby określić `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="6282c-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="6282c-105">[Poprzedni - metod kontrolera oraz widoki](controller-methods-views.md)
> [następne — Dodawanie pola](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="6282c-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
