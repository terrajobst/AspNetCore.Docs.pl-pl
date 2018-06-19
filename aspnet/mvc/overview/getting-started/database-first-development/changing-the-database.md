---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Baza danych EF najpierw o platformie ASP.NET MVC: Zmiana bazy danych | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 63ee8768a43dbdac80922e3adbedd3378c10da73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879325"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>Baza danych EF najpierw o platformie ASP.NET MVC: Zmiana bazy danych
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ta seria samouczka przedstawiono sposób automatycznego generowania kodu, która umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i Usuń dane, które znajdują się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na wprowadzenie aktualizacji struktury bazy danych i propagowanie tej zmiany w całej aplikacji sieci web.


## <a name="add-a-column"></a>Dodaj kolumnę

Po zaktualizowaniu struktura tabeli w bazie danych, musisz upewnij się, że zmiany są propagowane do modelu danych, widoków i kontrolera.

W tym samouczku doda nową kolumnę tabeli uczniów w celu rejestrowania drugie imię student. Aby dodać tę kolumnę, otwórz projekt bazy danych, a następnie otwórz plik Student.sql. Przy użyciu narzędzia Projektant lub kod T-SQL, Dodaj kolumnę o nazwie **MiddleName** który jest NVARCHAR(50) i umożliwia wartości NULL.

![Dodaj drugie imię](changing-the-database/_static/image1.png)

Wdróż tę zmianę z lokalną bazą danych, uruchamiając projekt bazy danych (lub F5). Nowe pole jest dodawane do tabeli. Jeśli użytkownik nie będzie widoczny w Eksploratorze obiektów SQL Server, kliknij przycisk Odśwież w okienku.

![Pokaż nowej kolumny](changing-the-database/_static/image2.png)

Nowa kolumna istnieje w tabeli bazy danych, ale jego obecnie nie istnieje w klasie modelu danych. Należy zaktualizować modelu w celu uwzględnienia nowej kolumny. W **modele** folder, otwórz **ContosoModel.edmx** plików do wyświetlenia na diagramie modelu. Zwróć uwagę, model uczniów nie zawiera właściwości MiddleName. Kliknij prawym przyciskiem myszy w dowolnym miejscu na powierzchni projektu i wybierz **modelu aktualizacji z bazy danych**.

![Aktualizuj model](changing-the-database/_static/image3.png)

W Kreatorze aktualizacji, wybierz **Odśwież** kartę i **uczniowie** tabeli.

![Kreator aktualizacji](changing-the-database/_static/image4.png)

Kliknij przycisk **Zakończ**.

Po zakończeniu procesu aktualizacji diagram zawiera nowe **MiddleName** właściwości. Zapisz **ContosoModel.edmx** pliku. Musisz zapisać ten plik nowej właściwości są propagowane do **Student.cs** klasy. Ma teraz zaktualizować bazę danych i modelu.

Skompiluj rozwiązanie.

Niestety widoki nadal nie zawierają nowej właściwości. Aby zaktualizować widoki dostępne są dwie opcje — można albo ponownie wygenerować widoki dodając ponownie funkcją szkieletów dla klasy dla użytkowników domowych, lub można ręcznie dodać nową właściwość do istniejących widoków. W tym samouczku doda rusztowania ponownie, ponieważ nie zostały wprowadzone zmiany dostosowanych widoków generowane automatycznie. Rozważ ręczne dodanie właściwości, gdy wprowadzono zmiany do widoków i nie chcesz utracić tych zmian.

W celu zapewnienia widoki są ponownie tworzone, Usuń **studentów** folder **widoków**i Usuń **StudentsController**. Następnie kliknij prawym przyciskiem myszy **kontrolerów** folderu i Dodaj funkcją szkieletów dla **uczniowie** modelu. Ponownie, nazwy kontrolera **StudentsController**. Wybierz **OK**.

Widoki zawiera teraz właściwości MiddleName.

![Pokaż drugie imię](changing-the-database/_static/image5.png)

W następnej sekcji dodasz kod, aby dostosować widok do wyświetlania szczegółów rekordu studenta.

> [!div class="step-by-step"]
> [Poprzednie](generating-views.md)
> [dalej](customizing-a-view.md)
