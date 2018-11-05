---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF bazy danych, najpierw z platformą ASP.NET MVC: zmienianie bazy danych | Dokumentacja firmy Microsoft'
author: Rick-Anderson
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 8d0eff37ced757cd8be74f8171c9aaa430940010
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021277"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>EF bazy danych, najpierw z platformą ASP.NET MVC: zmienianie bazy danych
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na wprowadzanie aktualizacji struktury bazy danych i propagowanie tej zmiany w całej aplikacji sieci web.


## <a name="add-a-column"></a>Dodaj kolumnę

Jeśli zaktualizujesz strukturę tabeli w bazie danych, należy upewnić się, Twoja zmiana jest propagowana do modelu danych, widoków i kontrolerów.

W tym samouczku dodasz nową kolumnę do tabeli dla uczniów, aby zarejestrować drugie imię studenta. Aby dodać tę kolumnę, otwórz projekt bazy danych, a następnie otwórz plik Student.sql. Za pomocą projektanta lub kodu T-SQL, Dodaj kolumnę o nazwie **MiddleName** , jest NVARCHAR(50) i dopuszcza wartości NULL.

![Dodaj drugie imię](changing-the-database/_static/image1.png)

Wdróż tę zmianę z lokalną bazą danych, uruchamiając projekt bazy danych (lub F5). Nowe pole jest dodawane do tabeli. Jeśli nie ma go w Eksploratorze obiektów SQL Server, kliknij przycisk Odśwież w okienku.

![Pokaż nowej kolumny](changing-the-database/_static/image2.png)

Nowa kolumna istnieje w tabeli bazy danych, ale ona obecnie nie istnieje w klasie modelu danych. Należy zaktualizować model w celu uwzględnienia nowej kolumny. W **modeli** folder, otwórz **ContosoModel.edmx** plik, aby wyświetlić diagram modelu. Zwróć uwagę, model dla uczniów nie zawiera właściwości MiddleName. Kliknij prawym przyciskiem myszy w dowolnym miejscu na powierzchni projektowej, a następnie wybierz pozycję **Model aktualizacji z bazy danych**.

![aktualizowanie modelu](changing-the-database/_static/image3.png)

W Kreatorze aktualizacji wybierz **Odśwież** kartę i **uczniów** tabeli.

![Kreator aktualizacji](changing-the-database/_static/image4.png)

Kliknij przycisk **Zakończ**.

Po zakończeniu procesu aktualizacji diagram zawiera nowe **MiddleName** właściwości. Zapisz **ContosoModel.edmx** pliku. Musisz zapisać ten plik nowej właściwości są propagowane do **Student.cs** klasy. Zostały teraz zaktualizowane bazy danych i modelu.

Skompiluj rozwiązanie.

Niestety widoki nadal nie zawierają nową właściwość. Aby zaktualizować widoki dostępne są dwie opcje — można ponownie wygenerować widoki dodając ponownie funkcją szkieletów dla klasy dla uczniów, lub można ręcznie dodawać nowych właściwości do istniejących widoków. W tym samouczku dodasz szkieletu ponownie, ponieważ nie wprowadzono zmian dostosowane widoki generowane automatycznie. Należy rozważyć, ręcznie dodając właściwość, jeśli wprowadzono zmiany do widoków i nie chcesz utracić te zmiany.

Aby upewnić się, widoki są ponownie tworzone, Usuń **studentów** folderze **widoków**i Usuń **StudentsController**. Następnie kliknij prawym przyciskiem myszy **kontrolerów** folderze i Dodaj funkcją szkieletów dla **uczniów** modelu. Ponownie, nazwy kontrolera **StudentsController**. Wybierz **OK**.

Widoki zawierają teraz właściwość MiddleName.

![Pokaż drugie imię](changing-the-database/_static/image5.png)

W następnej sekcji dodasz kod, aby dostosować widok do wyświetlania szczegółów rekordu dla uczniów.

> [!div class="step-by-step"]
> [Poprzednie](generating-views.md)
> [dalej](customizing-a-view.md)
