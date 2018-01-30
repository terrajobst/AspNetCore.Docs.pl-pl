---
title: Dodawanie wyszukiwania do platformy ASP.NET Core aplikacji MVC
author: rick-anderson
description: "Przedstawiono sposób dodawania wyszukiwania do prostej aplikacji platformy ASP.NET Core MVC"
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: d0e42242fd8c05b538e5e2e2686b77c95e9b90f4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

Uwaga: SQLlite jest uwzględniana wielkość liter, więc należy wyszukać "Duplikatu", a nie "duplikatu".

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

Zmień `<form>` tagów w *Views\movie\Index.cshtml* widoku Razor, aby określić `method="get"`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Poprzedni - metod kontrolera oraz widoki](controller-methods-views.md)
[następne — Dodawanie pola](new-field.md)
