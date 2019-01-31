---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Samouczek: Dostosowywanie widoku dla platformy EF Database First w aplikacji ASP.NET MVC'
description: Ten samouczek koncentruje się na zmianę widoki generowane automatycznie, aby zwiększyć prezentacji.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236500"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Samouczek: Dostosowywanie widoku dla platformy EF Database First w aplikacji ASP.NET MVC

Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.

Ten samouczek koncentruje się na zmianę widoki generowane automatycznie, aby zwiększyć prezentacji.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodaj kursy do strony szczegółów dla uczniów
> * Upewnij się, że kursy są dodawane do strony

## <a name="prerequisites"></a>Wymagania wstępne

* [Zmień bazę danych](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Dodaj kursy do szczegóły dotyczące ucznia

Wygenerowany kod zapewnia dobry punkt wyjścia dla twojej aplikacji, ale nie zawsze zapewnia ona wszystkich funkcji, które są potrzebne w Twojej aplikacji. Można dostosować kod w celu spełnienia wymagań konkretnej aplikacji. Obecnie aplikację nie są wyświetlane zarejestrowanych kursy dla wybranej uczniów lub studentów. W tej sekcji dodasz kursy zarejestrowanych dla każdego ucznia, aby **szczegóły** widoku dla uczniów lub studentów.

Otwórz **widoków** > **studentów** > *Details.cshtml*. Poniżej ostatniej &lt;/dl&gt; tagu, ale przed zamykającym &lt;/DIV&gt; tag, Dodaj następujący kod.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Ten kod tworzy tabelę, która wyświetla wiersz dla każdego rekordu w tabeli rejestracji dla wybranych uczniów lub studentów. **Wyświetlania** metoda powoduje renderowanie kodu HTML dla obiektu (elementu modelu), który reprezentuje wyrażenie. Metoda wyświetlania (a nie po prostu osadzania wartości właściwości w kodzie), aby upewnić się, czy wartość jest sformatowana poprawnie na podstawie jego typu i szablon dla tego typu. W tym przykładzie każde wyrażenie zwraca jedną właściwość z bieżącego rekordu w pętli, a wartości są typy pierwotne, które są renderowane jako tekst.

## <a name="confirm-courses-are-added"></a>Upewnij się, że są dodawane kursów

Uruchom rozwiązanie. Kliknij przycisk **listę uczniów** i wybierz **szczegóły** dla jednego z uczniów. Zobaczysz, że zarejestrowane kursy zostały uwzględnione w widoku.

![dla uczniów z rejestracją](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Następne kroki
W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodano kursy do strony szczegółów dla uczniów
> * Potwierdza, że kursy są dodawane do strony

Przejdź do następnego samouczka, aby dowiedzieć się, jak dodawać adnotacje danych do określania wymagań dotyczących walidacji i wyświetlenie formatowania.
> [!div class="nextstepaction"]
> [Ulepszanie weryfikacji danych](enhancing-data-validation.md)
