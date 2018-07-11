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

Możesz szybko zmienić nazwę `searchString` parametr `id` z **Zmień nazwę** polecenia. Kliknij prawym przyciskiem myszy `searchString` **> Zmień nazwę**.

![Menu kontekstowe](search/_static/rename.png)

Obiekty docelowe zmiany nazwy są wyróżnione.

![Edytor kodu, przedstawiający zmiennej wyróżnione w całym metoda ActionResult indeksu](search/_static/rename2.png)

Zmień parametr `id` i wszystkie wystąpienia `searchString` Zmień `id`.

![Edytor kodu, przedstawiający zmiennej został zmieniony na identyfikator](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

Zwróć uwagę, jak technologia intelliSense pomaga zaktualizować znaczników.

![Menu kontekstowe funkcji IntelliSense przy użyciu metody zaznaczony na liście atrybutów dla elementu form](search/_static/int_m.png)

![Menu kontekstowe IntelliSense z pobrać zaznaczony na liście wartości atrybutów — metoda](search/_static/int_get.png)

Zwróć uwagę, szczególne czcionkę w `<form>` tagu. Szczególne czcionki wskazuje tagu jest obsługiwana przez [pomocników tagów](~/mvc/views/tag-helpers/intro.md).

![tag formularza z tekstem purpurowy](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Poprzednie](controller-methods-views.md)
> [dalej](new-field.md)  
