---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Samouczek: Zmień bazę danych dla platformy EF Database First w aplikacji ASP.NET MVC'
description: Ten samouczek koncentruje się na wprowadzanie aktualizacji struktury bazy danych i propagowanie tej zmiany w całej aplikacji sieci web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667638"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>Samouczek: Zmień bazę danych dla platformy EF Database First w aplikacji ASP.NET MVC

Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.

Ten samouczek koncentruje się na wprowadzanie aktualizacji struktury bazy danych i propagowanie tej zmiany w całej aplikacji sieci web.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodaj kolumnę
> * Dodaj właściwość do widoków

## <a name="prerequisites"></a>Wymagania wstępne

* [Generowanie widoków](generating-views.md)

## <a name="add-a-column"></a>Dodaj kolumnę

Jeśli zaktualizujesz strukturę tabeli w bazie danych, należy upewnić się, Twoja zmiana jest propagowana do modelu danych, widoków i kontrolerów.

W tym samouczku dodasz nową kolumnę do tabeli dla uczniów, aby zarejestrować drugie imię studenta. Aby dodać tę kolumnę, otwórz projekt bazy danych, a następnie otwórz plik Student.sql. Za pomocą projektanta lub kodu T-SQL, Dodaj kolumnę o nazwie **MiddleName** , jest NVARCHAR(50) i dopuszcza wartości NULL.

Wdróż tę zmianę z lokalną bazą danych, uruchamiając projekt bazy danych (lub F5). Nowe pole jest dodawane do tabeli. Jeśli nie ma go w Eksploratorze obiektów SQL Server, kliknij przycisk Odśwież w okienku.

![Pokaż nowej kolumny](changing-the-database/_static/image2.png)

Nowa kolumna istnieje w tabeli bazy danych, ale ona obecnie nie istnieje w klasie modelu danych. Należy zaktualizować model w celu uwzględnienia nowej kolumny. W **modeli** folder, otwórz **ContosoModel.edmx** plik, aby wyświetlić diagram modelu. Zwróć uwagę, model dla uczniów nie zawiera właściwości MiddleName. Kliknij prawym przyciskiem myszy w dowolnym miejscu na powierzchni projektowej, a następnie wybierz pozycję **Model aktualizacji z bazy danych**.

W Kreatorze aktualizacji wybierz **Odśwież** , a następnie wybierz pozycję **tabel** > **dbo** > **uczniów**. Kliknij przycisk **Zakończ**.

Po zakończeniu procesu aktualizacji diagram zawiera nowe **MiddleName** właściwości. Zapisz **ContosoModel.edmx** pliku. Musisz zapisać ten plik nowej właściwości są propagowane do **Student.cs** klasy. Zostały teraz zaktualizowane bazy danych i modelu.

Skompiluj rozwiązanie.

## <a name="add-the-property-to-the-views"></a>Dodaj właściwość do widoków

Niestety widoki nadal nie zawierają nową właściwość. Aby zaktualizować widoki dostępne są dwie opcje — można ponownie wygenerować widoki dodając ponownie funkcją szkieletów dla klasy dla uczniów, lub można ręcznie dodawać nowych właściwości do istniejących widoków. W tym samouczku dodasz szkieletu ponownie, ponieważ nie wprowadzono zmian dostosowane widoki generowane automatycznie. Należy rozważyć, ręcznie dodając właściwość, jeśli wprowadzono zmiany do widoków i nie chcesz utracić te zmiany.

Aby upewnić się, widoki są ponownie tworzone, Usuń **studentów** folderze **widoków**i Usuń **StudentsController**. Następnie kliknij prawym przyciskiem myszy **kontrolerów** folderze i Dodaj funkcją szkieletów dla **uczniów** modelu. Ponownie, nazwy kontrolera **StudentsController**. Wybierz pozycję **Dodaj**.

Ponownie skompiluj rozwiązanie. Widoki zawierają teraz właściwość MiddleName.

![Pokaż drugie imię](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodano kolumnę
> * Dodaje właściwość do widoków

Przejdź do następnego samouczka, aby dowiedzieć się, jak dostosować widok do wyświetlania szczegółów rekordu dla uczniów.
> [!div class="nextstepaction"]
> [Dostosowywanie widoku](customizing-a-view.md)