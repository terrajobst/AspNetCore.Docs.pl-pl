---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Tworzenie bazy danych | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 0e72359095e4c40ef7e56f1290a45ded257143c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362359"
---
<a name="creating-a-database"></a>Tworzenie bazy danych
====================
przez [Scotta Hanselmana](https://github.com/shanselman)

> Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.


W tej sekcji użyjemy do utworzenia nowego programu SQL Express bazy danych, która użyjemy do przechowywania i pobierania danych filmu. Z poziomu programu Visual Web Developer środowiska IDE, wybierz widok | Eksploratora serwera. Kliknij prawym przyciskiem myszy połączeń danych, a następnie kliknij przycisk Dodaj połączenie...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

W oknie dialogowym Wybierz źródło danych wybierz program Microsoft SQL Server, a następnie wybierz pozycję Kontynuuj.

![](getting-started-with-mvc-part4/_static/image2.png)

W oknie dialogowym Dodaj połączenie, wprowadź ". \SQLEXPRESS" dla nazwy serwera, a następnie wprowadź "Filmy" jako nazwę nowej bazy danych.

[![Dodaj okno dialogowe połączenia](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Kliknij przycisk OK, a użytkownik zostanie zapytany, jeśli chcesz utworzyć tę bazę danych. Kliknij przycisk Tak.

[![Utwórz filmy?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Teraz masz pustej bazy danych w Eksploratorze serwera.

![Dodaj nową tabelę](getting-started-with-mvc-part4/_static/image7.png)

Kliknij prawym przyciskiem myszy w tabelach, a następnie kliknij przycisk Dodaj tabelę. Pojawi się okno projektanta tabel. Dodaj kolumny Identyfikator, tytuł, ReleaseDate, gatunku i ceny. Kliknij prawym przyciskiem myszy kolumny Identyfikatora, a następnie kliknij polecenie Ustaw klucz podstawowy. Oto mój projekt obszary wygląda podobnie.

[![Edytor tabel bazy danych](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Ponadto wybierz kolumny identyfikatora, a w obszarze poniżej właściwości kolumny zmiany "Specyfikacja tożsamości", "Yes".

[![IsIdentity — właściwości kolumny](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Jeśli właśnie tak zrobić, kliknij ikonę Zapisz na pasku narzędzi lub wybierz plik | Zapisz z menu, a nazwa tabeli "**filmu**" (pojedynczą). Gdy mamy już bazę danych i tabelę!

[![Wybierz nazwę](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Wróć do Eksploratora serwera i kliknij prawym przyciskiem myszy tabelę filmu, a następnie wybierz opcję "Pokaż dane tabeli". Wprowadź kilka filmów, więc niektóre dane nie naszej bazie danych.

[![Edytowanie tabel bazy danych](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Tworzenie modelu

Teraz przejdź z powrotem do Eksploratora rozwiązań na po prawej stronie okna środowiska IDE programu i kliknij prawym przyciskiem myszy w folderze modeli i wybierz opcję Dodaj | Nowy element.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Zamierzamy utworzyć Model jednostki z naszej nowej bazy danych. Zestaw klas, spowoduje to dodanie do naszego projektu, który ułatwia to firmie Microsoft w celu wykonywania zapytań i manipulowania danymi w naszej bazie danych. Wybierz węzeł danych po lewej stronie okna dialogowego, a następnie wybierz szablon elementu ADO.NET Entity Data Model. Nadaj mu nazwę Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Kliknij przycisk "Dodaj". Spowoduje to uruchomienie "Kreator modelu danych jednostki".

W oknie dialogowym Nowy, które się pojawi wybierz pozycję Generuj z bazy danych. Ponieważ właśnie wprowadziliśmy bazę danych, będziemy potrzebować tylko mówić Entity Framework na temat naszej nowej bazy danych i jego tabeli. Kliknij obok pozycji Zapisz naszych połączenia z bazą danych w konfiguracji naszej aplikacji sieci web. Sprawdź tabele i film pole wyboru, a następnie kliknij przycisk Zakończ.

[![Kreator modelu Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Teraz możemy Zobacz naszej nowej tabeli filmu w Entity Framework Designer i uzyskać do niego dostęp z poziomu kodu.

[![Filmy — Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Na powierzchni projektowej widać klasy "Filmu". Ta klasa jest mapowany do tabeli "Filmu" w naszej bazie danych i każdej właściwości w nim mapuje kolumną z tabeli. Każde wystąpienie klasy "Filmu" odpowiada wiersz w tabeli "Filmu".

Jeśli nie potrzebujesz domyślne nazewnictwa i Konwencji używanych przez program Entity Framework mapowania, może używać Projektanta Entity Framework, zmienić lub dostosować je. Dla tej aplikacji będzie używane wartości domyślne i mogą po prostu zapisz plik jako-to.

Teraz Użyjmy niektórych rzeczywistych danych!

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part3.md)
> [dalej](getting-started-with-mvc-part5.md)
