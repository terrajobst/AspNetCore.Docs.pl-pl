---
title: Dodawanie wyszukiwania
author: rick-anderson
description: Przedstawiono sposób dodawania wyszukiwania do prostej aplikacji platformy ASP.NET Core MVC
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: aee1682755385d9fa292f9ba0814d5d3602f3881
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729911"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

Można szybko zmienić `searchString` parametr `id` z **zmienić** polecenia. Kliknij prawym przyciskiem myszy `searchString` **> Zmień nazwę**.

![Menu kontekstowe](search/_static/rename.png)

Elementy docelowe rename są wyróżnione.

![Edytor kodu przedstawiający zmiennej wyróżniane w metodzie ActionResult indeksu](search/_static/rename2.png)

Zmień parametr `id` i wszystkie wystąpienia `searchString` Zmień `id`.

![Edytor kodu przedstawiający zmiennej został zmieniony na identyfikator](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

Zwróć uwagę, jak intelliSense pomaga nam zaktualizuj kod znaczników.

![Menu kontekstowe IntelliSense z zaznaczony na liście atrybutów dla elementu form — metoda](search/_static/int_m.png)

![Menu kontekstowe IntelliSense z pobrać zaznaczony na liście wartości atrybutów — metoda](search/_static/int_get.png)

Zwróć uwagę, charakterystyczne czcionkę w `<form>` tagu. Charakterystyczne czcionki wskazuje tagu jest obsługiwana przez [pomocników tagów](~/mvc/views/tag-helpers/intro.md).

![tag formularza o purpurowa tekstu](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Poprzednie](controller-methods-views.md)
> [dalej](new-field.md)  
