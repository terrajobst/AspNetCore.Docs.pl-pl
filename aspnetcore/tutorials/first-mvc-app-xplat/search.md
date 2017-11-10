---
title: Dodawanie wyszukiwania
author: rick-anderson
description: "Przedstawiono sposób dodawania wyszukiwania do prostej aplikacji platformy ASP.NET Core MVC"
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-ffff-4628-855d-200206d96269
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 57419c697aea80a054e906c75002f5a39c512fc6
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/11/2017
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
