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
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

Można szybko zmienić `searchString` parametr `id` z **zmienić** polecenia. Kliknij prawym przyciskiem myszy `searchString` **> Zmień nazwę**.

![Menu kontekstowe](search/_static/rename.png)

Elementy docelowe rename są wyróżnione.

![Edytor kodu przedstawiający zmiennej wyróżniane w metodzie ActionResult indeksu](search/_static/rename2.png)

Zmień parametr `id` i wszystkie wystąpienia `searchString` Zmień `id`.

![Edytor kodu przedstawiający zmiennej został zmieniony na identyfikator](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

Zwróć uwagę, jak intelliSense pomaga nam zaktualizuj kod znaczników.

![Menu kontekstowe IntelliSense z zaznaczony na liście atrybutów dla elementu form — metoda](search/_static/int_m.png)

![Menu kontekstowe IntelliSense z pobrać zaznaczony na liście wartości atrybutów — metoda](search/_static/int_get.png)

Zwróć uwagę, charakterystyczne czcionkę w `<form>` tagu. Charakterystyczne czcionki wskazuje tagu jest obsługiwana przez [pomocników tagów](../../mvc/views/tag-helpers/intro.md).

![tag formularza o purpurowa tekstu](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Poprzednie](controller-methods-views.md)
[dalej](new-field.md)  
