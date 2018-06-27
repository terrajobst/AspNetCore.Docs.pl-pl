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
