---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF bazy danych, najpierw z platformą ASP.NET MVC: Dostosowywanie widoku | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: ce450af93459f2a69557b3fe0d1ead813ae99986
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752771"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>EF bazy danych, najpierw z platformą ASP.NET MVC: Dostosowywanie widoku
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na zmianę widoki generowane automatycznie, aby zwiększyć prezentacji.


## <a name="add-enrolled-courses-to-student-details"></a>Dodaj zarejestrowanych kursów do szczegółów dla uczniów

Wygenerowany kod zapewnia dobry punkt wyjścia dla twojej aplikacji, ale nie zawsze zapewnia ona wszystkich funkcji, które są potrzebne w Twojej aplikacji. Można dostosować kod w celu spełnienia wymagań konkretnej aplikacji. Obecnie aplikację nie są wyświetlane zarejestrowanych kursy dla wybranej uczniów lub studentów. W tej sekcji dodasz kursy zarejestrowanych dla każdego ucznia, aby **szczegóły** widoku dla uczniów lub studentów.

Otwórz **Students/Details.cshtml**, a poniżej ostatniej &lt;/dl&gt; karcie, ale przed zamykającym &lt;/DIV&gt; tag, Dodaj następujący kod.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Ten kod tworzy tabelę, która wyświetla wiersz dla każdego rekordu w tabeli rejestracji dla wybranych uczniów lub studentów. **Wyświetlania** metoda powoduje renderowanie kodu HTML dla obiektu (elementu modelu), który reprezentuje wyrażenie. Metoda wyświetlania (a nie po prostu osadzania wartości właściwości w kodzie), aby upewnić się, czy wartość jest sformatowana poprawnie na podstawie jego typu i szablon dla tego typu. W tym przykładzie każde wyrażenie zwraca jedną właściwość z bieżącego rekordu w pętli, a wartości są typy pierwotne, które są renderowane jako tekst.

Przejdź do widoku studentów/Index ponownie, a następnie wybierz pozycję **szczegóły** dla jednego z uczniów. Zobaczysz, że zarejestrowane kursy zostały uwzględnione w widoku.

![dla uczniów z rejestracją](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Poprzednie](changing-the-database.md)
> [dalej](enhancing-data-validation.md)
