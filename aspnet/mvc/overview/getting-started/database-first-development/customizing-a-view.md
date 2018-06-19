---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Baza danych EF najpierw o platformie ASP.NET MVC: dostosowywanie widok | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 8338603e032329ad03d47c6392e508aa07c6858e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867661"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>Baza danych EF najpierw o platformie ASP.NET MVC: Dostosowywanie widoku
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ta seria samouczka przedstawiono sposób automatycznego generowania kodu, która umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i Usuń dane, które znajdują się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na zmienianie widoków automatycznie generowane w celu zwiększenia prezentacji.


## <a name="add-enrolled-courses-to-student-details"></a>Dodaj kursy zarejestrowanych do szczegółów dla użytkowników domowych

Wygenerowany kod zapewnia dobry punkt wyjścia dla aplikacji, ale nie zawsze zapewnia ona wszystkich funkcji, które są potrzebne w aplikacji. Można dostosować kodu w celu spełnienia wymagań konkretnej aplikacji. Obecnie aplikacji nie są wyświetlane szkoleń w zarejestrowany dla uczniów wybrane. W tej sekcji dodasz kursy zarejestrowanych dla użytkowników do **szczegóły** widok studenta.

Otwórz **Students/Details.cshtml**i poniżej ostatniego &lt;/dl&gt; kartę, ale przed zamknięciem &lt;/DIV&gt; tagów, Dodaj następujący kod.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Ten kod tworzy tabelę, która wyświetla wiersz dla każdego rekordu w tabeli rejestracji dla uczniów wybrane. **Wyświetlania** metoda powoduje renderowanie kodu HTML dla obiekt (modelItem), który reprezentuje wyrażenie. Metoda wyświetlania (zamiast po prostu osadzanie wartości właściwości w kodzie) do upewnij się, że wartość jest sformatowany poprawnie na podstawie typu i szablon dla tego typu. W tym przykładzie każde wyrażenie zwraca jedną właściwość z bieżącego rekordu w pętli i wartości są typy pierwotne, które mają być renderowane jako tekst.

Przejdź do widoku studentów/indeksu ponownie, a następnie wybierz **szczegóły** jednego studentów. Zobaczysz, że zarejestrowane kursy zostały uwzględnione w widoku.

![uczniów przy rejestracji.](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Poprzednie](changing-the-database.md)
> [dalej](enhancing-data-validation.md)
