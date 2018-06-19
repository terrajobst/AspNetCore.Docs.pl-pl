---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Tworzenie bazy danych | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utwórz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: ff2a41803cd31ce50bbf79e630d827b6de441ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868077"
---
<a name="creating-a-database"></a>Tworzenie bazy danych
====================
przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.


W tej sekcji zamierzamy utworzyć nowe SQL Express bazy danych, która zostanie użyta do przechowywania i pobierania danych filmu. W ramach programu Visual Web Developer środowiska IDE zaznacz widok | Eksploratora serwera. Kliknij prawym przyciskiem myszy połączenia danych, a następnie kliknij przycisk Dodaj połączenia...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

W oknie dialogowym Wybierz źródło danych wybierz pozycję Microsoft SQL Server i wybierz Kontynuuj.

![](getting-started-with-mvc-part4/_static/image2.png)

W oknie dialogowym Dodawanie połączenia, wprowadź ". \SQLEXPRESS" dla nazwy serwera, a następnie wprowadź nazwę nowej bazy danych "Filmy".

[![Dodaj połączenie, okno dialogowe](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Kliknij przycisk OK, a użytkownik zostanie zapytany, jeśli chcesz utworzyć tę bazę danych. Kliknij przycisk Tak.

[![Tworzenie filmów?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Teraz otrzymasz pustej bazy danych w Eksploratorze serwera.

![Dodaj nową tabelę](getting-started-with-mvc-part4/_static/image7.png)

Kliknij prawym przyciskiem myszy w tabelach, a następnie kliknij przycisk Dodaj tabelę. Pojawi się projektanta tabel. Dodawanie kolumn dla identyfikatora, tytułu, ReleaseDate, Genre i cenę. Kliknij prawym przyciskiem myszy w kolumnie identyfikator i kliknij przycisk Ustaw klucz podstawowy. Oto wygląda jak Moje obszary projektu.

[![Edytor tabel bazy danych](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Ponadto wybierz w kolumnie identyfikator i w obszarze poniżej właściwości kolumny Zmień "Specyfikacji tożsamości" na "Tak".

[![IsIdentity - właściwości kolumny](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Po skonfigurowaniu pracy, kliknij ikonę Zapisz na pasku narzędzi lub wybierz plik | Z menu i nazwy tabeli "**film**" (w liczbie pojedynczej). Mamy bazę danych i tabeli!

[![Wybierz nazwę](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Wróć do Eksploratora serwera i tabeli filmu kliknij prawym przyciskiem myszy, a następnie wybierz opcję "Pokaż dane tabeli". Wprowadź kilka filmów, więc niektóre dane nie naszej bazie danych.

[![Edytowanie tabeli bazy danych](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Tworzenie modelu

Teraz, wrócić do Eksploratora rozwiązań po prawej stronie IDE i kliknij prawym przyciskiem folder modeli i wybierz opcję Dodaj | Nowy element.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Zamierzamy utworzyć modelu jednostki z naszej nowej bazy danych. Spowoduje to dodanie zestaw klas naszych projekt, który ułatwia firmie Microsoft w celu wykonywania zapytań i manipulowanie danymi w naszej bazie danych. Wybierz węzeł danych po lewej stronie okna dialogowego, a następnie wybierz szablon elementu modelu danych jednostki ADO.NET. Nadaj mu nazwę Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Kliknij przycisk "Dodaj". Następnie uruchamiają "kreatora modelu danych jednostki".

W oknie dialogowym Nowy, które pojawia się wybierz opcję generowania z bazy danych. Ponieważ właśnie wprowadziliśmy bazy danych, potrzebujemy tylko Opisz programu Entity Framework w naszej nowej bazy danych i tabeli. Kliknij przycisk Dalej, aby zapisać naszych połączenia z bazą danych w konfiguracji naszej aplikacji sieci web. Sprawdź teraz, tabele i filmu wyboru i kliknij przycisk Zakończ.

[![Kreator modelu danych jednostki](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Po wprowadzeniu Zobacz nasze nową tabelę filmu Entity Framework Designer i uzyskać do niego dostęp z kodu.

[![Filmów - Microsoft Visual Web Developer 2010 Express.](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Na powierzchni projektowej widać klasy "Filmu". Ta klasa mapowany na tabelę "Filmu" w naszej bazie danych i każdej właściwości w niej mapowanym na kolumnę z tabeli. Każde wystąpienie klasy "Filmu" odpowiada wiersza w tabeli "Filmu".

Jeśli nie lubisz domyślnie i mapowanie konwencje używane przez program Entity Framework, można użyć programu Entity Framework designer można zmienić lub dostosować je. Dla tej aplikacji będzie możemy użyć wartości domyślnych i zapisać go jako — jest.

Teraz załóżmy działają niektóre dane!

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part3.md)
> [dalej](getting-started-with-mvc-part5.md)
